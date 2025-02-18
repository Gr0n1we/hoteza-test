<script setup lang="ts">
import type { ForecastData } from '@/type/ForecastData'
import type { PropType } from 'vue'

const props = defineProps({
  cityDetails: {
    type: Object as PropType<ForecastData>,
    required: true,
  },
})
</script>

<template>
  <div
    class="p-[16px] gap-[6px] flex flex-col text-white text-[14px] leading-[20px] bg-[#0E2E37] rounded-[8px]"
  >
    <div>
      Exploring: <span class="font-medium">{{ props.cityDetails.cityName }}</span>
    </div>
    <div>
      Country: <span class="font-medium">{{ props.cityDetails.country }}</span>
    </div>
    <div>
      Current temperature: <span class="font-medium">{{ props.cityDetails.temperature }}°C</span>
    </div>
  </div>
</template>
