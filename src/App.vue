<script setup lang="ts">
import { computed, ref } from 'vue'
import { Listbox, ListboxButton, ListboxOption, ListboxOptions } from '@headlessui/vue'

const fromCurrency = ref('USD')
const sendAmount = ref('1000')
const toCurrency = ref('EUR')
const receiveAmount = ref('900')

const currencies = [
  { code: 'EUR', name: 'Euro', imageCode: 'eu' },
  { code: 'USD', name: 'US Dollar', imageCode: 'us' },
  { code: 'GBP', name: 'British Pound', imageCode: 'gb' },
  { code: 'JPY', name: 'Japanese Yen', imageCode: 'jp' },
  { code: 'CAD', name: 'Canadian Dollar', imageCode: 'ca' },
  { code: 'AUD', name: 'Australian Dollar', imageCode: 'au' },
  { code: 'CHF', name: 'Swiss Franc', imageCode: 'ch' },
  { code: 'CNY', name: 'Chinese Yuan', imageCode: 'cn' },
  { code: 'SEK', name: 'Swedish Krona', imageCode: 'se' },
  { code: 'NZD', name: 'New Zealand Dollar', imageCode: 'nz' },
]

const selectedFromCurrency = computed(() => {
  return currencies.find((c) => c.code === fromCurrency.value)
})
const selectedToCurrency = computed(() => {
  return currencies.find((c) => c.code === toCurrency.value)
})

const getFlagUrl = (imageCode: string) => {
  return new URL(`./assets/images/flags/${imageCode}.webp`, import.meta.url).href
}
</script>

<template>
  <div class="main-container bg-neutral-600">
    <header class="p-4">
      <img src="@/assets/images/logo.svg" alt="Foreign Exchange Checker Logo" />
    </header>

    <main class="p-4">
      <section aria-labelledby="converter-heading">
        <h2 id="converter-heading" class="font-semibold text-neutral-50 text-xl">CHECK THE RATE</h2>

        <form action="/converter" method="post" class="bg-neutral-500 p-4 rounded-2xl flex flex-col gap-4">
          <div class="bg-neutral-400 p-4 rounded-2xl">
            <p class="text-neutral-100">SEND</p>
            <div class="flex justify-between">
              <input
                type="number"
                v-model="sendAmount"
                class="text-neutral-50"
                placeholder="1000"
              />
              <Listbox as="div" v-model="fromCurrency" class="bg-neutral-300 p-2.5 rounded-2xl">
                <div class="flex items-center gap-2">
                  <img
                    :src="getFlagUrl(selectedFromCurrency?.imageCode || '')"
                    :alt="selectedFromCurrency?.name"
                    class="w-5 h-5 rounded-2xl"
                  />
                  <ListboxButton class="text-neutral-50">{{ fromCurrency }}</ListboxButton>
                  <img
                    src="`@/assets/images/icon-chevron-down.svg`"
                    alt="US Flag"
                    class="w-5 h-5 rounded-full"
                  />
                </div>
                <ListboxOptions class="">
                  <ListboxOption
                    class="text-neutral-50"
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
            </div>
          </div>

          <div class="bg-neutral-400 p-4 rounded-2xl">
            <p class="text-neutral-100">RECEIVE</p>
            <div class="flex justify-between">
              <input
                type="number"
                v-model="receiveAmount"
                class="text-neutral-50"
                placeholder="1000"
              />
              <Listbox as="div" v-model="toCurrency" class="bg-neutral-300 p-2.5 rounded-2xl">
                <div class="flex items-center gap-2">
                  <img
                    :src="getFlagUrl(selectedToCurrency?.imageCode || '')"
                    :alt="selectedToCurrency?.name"
                    class="w-5 h-5 rounded-2xl"
                  />
                  <ListboxButton class="text-neutral-50">{{ toCurrency }}</ListboxButton>
                  <img
                    src="`@/assets/images/icon-chevron-down.svg`"
                    alt="US Flag"
                    class="w-5 h-5 rounded-full"
                  />
                </div>
                <ListboxOptions class="">
                  <ListboxOption
                    class="text-neutral-50"
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
            </div>
          </div>

          
        </form>
      </section>

      <section aria-labelledby="history-heading">
        <dl>
          <dt>EUR/USD</dt>
        </dl>
      </section>
    </main>
  </div>
</template>

<style scoped></style>
