<script setup lang="ts">
import { ref } from 'vue'
import { Listbox, ListboxButton, ListboxOption, ListboxOptions } from '@headlessui/vue'

const fromCurrency = ref('USD')
const sendAmount = ref('1000')

const currencies = [
  { code: 'EUR', name: 'Euro' },
  { code: 'USD', name: 'US Dollar' },
  { code: 'GBP', name: 'British Pound' },
  { code: 'JPY', name: 'Japanese Yen' },
  { code: 'CAD', name: 'Canadian Dollar' },
  { code: 'AUD', name: 'Australian Dollar' },
  { code: 'CHF', name: 'Swiss Franc' },
  { code: 'CNY', name: 'Chinese Yuan' },
  { code: 'SEK', name: 'Swedish Krona' },
  { code: 'NZD', name: 'New Zealand Dollar' },
]
</script>

<template>
  <div class="main-container bg-neutral-600">
    <header class="p-4">
      <img src="@/assets/images/logo.svg" alt="Foreign Exchange Checker Logo">
    </header>

    <main class="p-4">
      <section aria-labelledby="converter-heading">
        <h2 id="converter-heading" class="font-semibold text-neutral-50 text-xl">CHECK THE RATE</h2>

        <form action="/converter" method="post" class="bg-neutral-500 p-4 rounded-2xl">
          <div class="bg-neutral-400 p-4 rounded-2xl">
            <p class="text-neutral-100">SEND</p>
            <div class="flex justify-between">
              <input type="number" v-model="sendAmount" class="text-neutral-50" placeholder="1000">
                <Listbox as="div" v-model="fromCurrency" class="bg-neutral-300 p-2.5 rounded-2xl">
                  <div class="flex items-center gap-2">
                    <img src="`@/assets/images/flags/us.webp`" alt="US Flag" class="w-5 h-5 rounded-2xl">
                    <ListboxButton class="text-neutral-50">{{ fromCurrency }}</ListboxButton>
                    <img src="`@/assets/images/icon-chevron-down.svg`" alt="US Flag" class="w-5 h-5 rounded-full">
                  </div>
                  <ListboxOptions class="">
                    <ListboxOption class="text-neutral-50"
                    v-for="currency in currencies"
                    :key="currency.code"
                    :value="currency.code"
                    >
                    {{ currency.code }}
                  </ListboxOption>
                </ListboxOptions>
              </Listbox>
            </div>
          </div>



          <div>
            <label for="amount">Amount</label>
            <input type="number" id="amount" />
          </div>

          <div>
            <button>Convert</button>
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
