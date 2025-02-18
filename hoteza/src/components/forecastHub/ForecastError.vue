<script setup lang="ts">
const props = defineProps({ message: String })
</script>

<template>
  <div class="text-[#FF2D2D] text-[14px] leading-[20px] text-center font-medium">{{ message }}</div>
</template>
