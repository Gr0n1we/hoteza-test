//Тестовое задание. Получение текущей погоды по названию города. //Релизовать на странице input для
ввода названия города на английском и кнопку отправки данных. //Получить данные о долготе, широте и
названии страны через API https://geocoding-api.open-meteo.com/v1/search?name={city}&count=1
//Объект с latitude, longitude, country - API_RESPONSE.results[0] //Получить данные о погоде через
API
https://api.open-meteo.com/v1/forecast?latitude={latitude}&longitude={longitude}&current_weather=true&timezone=auto
//Погода - API_RESPONSE.current_weather.temperature //Отобразить полученные данные вместе с
названием города и страны. //При ошибке API отобразить текст "City not found", если инпут пустой -
"Please enter the city" (при таком раскладе запросы не должны отправляться), при любых изменениях в
input ошибка должна пропадать. //Доп: макет в figma:
https://www.figma.com/design/PgLn2ZvwCGpZtH8ZxDUQw2/Hoteza---Frontend-developer-test-task?node-id=0-1&t=GTiIFnB5byl50Iqx-1
//Верстка на tailwind будет преимуществом

<script setup lang="ts">
import { ref } from 'vue'
import ForecastForm from '@/components/forecastHub/ForecastForm.vue'
import ForecastError from '@/components/forecastHub/ForecastError.vue'
import ForecastDetails from '@/components/forecastHub/ForecastDetails.vue'
import type { ForecastData } from '@/type/ForecastData'

const cityError = ref({
  status: false,
  message: '',
})

const cityDetails = ref<ForecastData>({
  country: '',
  temperature: '',
  cityName: '',
})

const getWeather = async (cityName: string) => {
  if (cityName == '') {
    cityError.value.status = true
    cityError.value.message = 'Please enter the city'
    return
  }

  try {
    const geoResponse = await fetch(
      `https://geocoding-api.open-meteo.com/v1/search?name=${cityName}&count=1`,
    )
    const geoData = await geoResponse.json()
    const geoDataObj = geoData.results[0]
    cityDetails.value.country = geoDataObj.country

    const weatherResponse = await fetch(
      `https://api.open-meteo.com/v1/forecast?latitude=${geoDataObj.latitude}&longitude=${geoDataObj.longitude}&current_weather=true&timezone=auto`,
    )
    const weatherData = await weatherResponse.json()
    cityDetails.value.temperature = weatherData.current_weather.temperature
    cityDetails.value.cityName = cityName
  } catch (e) {
    cityError.value.status = true
    cityError.value.message = 'The city you entered is not correct'
  }
}

const errorReset = () => {
  cityError.value.status = false
  cityError.value.message = ''
  cityDetails.value.country = ''
  cityDetails.value.temperature = ''
  cityDetails.value.cityName = ''
}
</script>

<template>
  <div class="gap-[32px] flex flex-col">
    <ForecastForm @getWeather="getWeather" @errorReset="errorReset" :isError="cityError.status" />
    <ForecastDetails
      v-if="!cityError.status && cityDetails.temperature"
      :cityDetails="cityDetails"
    />
    <ForecastError v-if="cityError.status" :message="cityError.message" />
  </div>
</template>
