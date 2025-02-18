Получение погоды по названию города

В компоненте ForecastForm.vue реализовал форму с input для ввода названия города. При нажатии на кнопку в этом компоненте выполняется emit getWeather с передачей названия города.

В компоненте ForecastHub.vue при получении эмита getWeather выполняется метод getWeather. В методе getWeather выполняется проверка на то, является ли значение input пустой строкой: 

if (cityName == '') {
    cityError.value.status = true
    cityError.value.message = 'Please enter the city'
    return
  }

В случае если пользователь ввел название города, то через API (https://geocoding-api.open-meteo.com/v1/search?name=${cityName}&count=1) получается долгота, широта и страна.

Далее по долготе и широте через API (https://api.open-meteo.com/v1/forecast?latitude=${geoDataObj.latitude}&longitude=${geoDataObj.longitude}&current_weather=true&timezone=auto) определяется текущая погода в городе. Данные записываются в объект cityDetails.

Для объекта cityDetails используется типизация:

export interface ForecastData {
  country: string
  temperature: string
  cityName: string
}

Объект передается через props в компонент ForecastDetails.vue, где они отображаются.

Для обработки ошибок использую объект cityError, который содержит статус и сообщение для пользователя. Для отображения ошибок, используется компонент ForecastError.vue.

При вводе любого значения в input из компанента ForecastForm.vue выполнится emit errorReset после чего, cityError сбросится и ошибки перестанут отображаться.
