<script setup lang="ts">
import { defineModel, computed } from 'vue'

const amount = defineModel<string>()

const displayValue = computed({
  get() {
    return amount.value ? formatNumber(amount.value) : ''
  },
  set(value: string) {
    const cleanNumber = value.replace(/,/g, '')
    amount.value = cleanNumber
  },
})

const formatNumber = (value: number | string) => {
  const num = Number(value)
  if (isNaN(num)) return ''
  return new Intl.NumberFormat('en-US').format(num)
}
</script>

<template>
  <input
    type="text"
    inputmode="decimal"
    v-model="displayValue"
    class="text-[32px] w-full min-w-0"
    placeholder="1,000"
  />
</template>
