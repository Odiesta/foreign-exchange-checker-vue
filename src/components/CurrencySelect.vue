<script setup lang="ts">
import { computed } from 'vue'
import { Listbox, ListboxButton, ListboxOptions, ListboxOption } from '@headlessui/vue'
const model = defineModel<string>()
const props = defineProps({ currencies: Array<{ code: string; name: string; imageCode: string }> })

const selectedCurrency = computed(() => {
  return props.currencies?.find((c: { code: string }) => c.code === model.value)
})

const getFlagUrl = (imageCode: string) => {
  return new URL(`../assets/images/flags/${imageCode}.webp`, import.meta.url).href
}
</script>

<template>
  <Listbox as="div" v-model="model" class="bg-neutral-300 p-2.5 rounded-2xl relative">
    <div class="flex items-center gap-2">
      <ListboxButton class="text-neutral-50 flex items-center gap-2">
        <img
          :src="getFlagUrl(selectedCurrency?.imageCode || '')"
          :alt="selectedCurrency?.name"
          class="w-5 h-5 rounded-2xl"
        />{{ modelValue
        }}<img
          src="`@/assets/images/icon-chevron-down.svg`"
          alt="US Flag"
          class="w-5 h-5 rounded-full"
        />
      </ListboxButton>
    </div>
    <ListboxOptions
      class="p-2.5 -mt-2 absolute top-full left-0 right-0 z-10 h-32 overflow-y-auto bg-neutral-300 rounded-b-2xl"
    >
      <ListboxOption
        class="text-neutral-50 flex items-center gap-2 py-1"
        v-for="currency in currencies"
        :key="currency.code"
        :value="currency.code"
      >
        <img
          :src="getFlagUrl(currency.imageCode)"
          :alt="`${currency.name} Flag`"
          class="w-5 h-5 rounded-full"
        />{{ currency.code }}
      </ListboxOption>
    </ListboxOptions>
  </Listbox>
</template>
