<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps({
  isError: Boolean,
})
const cityName = ref('')
const emit = defineEmits(['getWeather', 'errorReset'])
</script>

<template>
  <div class="w-[360px] gap-[32px] flex flex-col">
    <div class="w-full flex flex-col items-center justify-between gap-[24px]">
      <img src="@/assets/Logomark.svg" />
      <h1 class="text-[30px] text-white font-semibold leading-[38px]">Forecast Hub</h1>
    </div>
    <div class="gap-[24px] flex flex-col">
      <div class="relative">
        <input
          :class="{
            'border-[#FF2D2D]': props.isError,
            'border-[#98A2B3] focus:border-white not-placeholder-shown:border-white':
              !props.isError,
          }"
          v-model="cityName"
          @input="emit('errorReset')"
          class="bg-transparent border-b-1 leading-[24px] text-[#FFFFFF] font-regular text-[16px] placeholder:font-regular placeholder:text-[16px] placeholder:text-[#98A2B3] w-full pt-[10px] pb-[9px] transition-all duration-200 ease-in-out"
          placeholder="Enter your city"
        />
        <div
          v-if="props.isError && !cityName"
          class="absolute right-0 top-[10px] text-[16px] text-[#FF2D2D] leading-[24px]"
        >
          *
        </div>
      </div>
      <button
        @click="emit('getWeather', cityName)"
        class="bg-[#257187] text-[#FFFFFF] text-[16px] leading-[24px] py-[10px] px-[20px] font-semibold rounded-[8px]"
      >
        Weather Now
      </button>
    </div>
  </div>
</template>
