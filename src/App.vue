<script setup lang="ts">
import { Listbox, ListboxButton, ListboxOption, ListboxOptions } from '@headlessui/vue';
const currencies = [
  {code: 'EUR', name: 'Euro'},
  {code: 'USD', name: 'US Dollar'},
  {code: 'GBP', name: 'British Pound'},
  {code: 'JPY', name: 'Japanese Yen'},
  {code: 'CAD', name: 'Canadian Dollar'},
  {code: 'AUD', name: 'Australian Dollar'},
  {code: 'CHF', name: 'Swiss Franc'},
  {code: 'CNY', name: 'Chinese Yuan'},
  {code: 'SEK', name: 'Swedish Krona'},
  {code: 'NZD', name: 'New Zealand Dollar'},
]

</script>

<template>
  <div class="container ">
    <header>

    </header>

    <main>
      <section aria-labelledby="converter-heading">
        <h2 id="converter-heading" class="sr-only">Currency Converter</h2>

        <form action="/converter" method="post" class="flex flex-col items-center space-y-4">
          <Listbox v-model="fromCurrency" name="currency-from" id="currency-from">
            <ListboxButton>{{ fromCurrency }}</ListboxButton>
            <ListboxOptions>
              <ListboxOption v-for="currency in currencies" :key="currency.code" :value="currency.code">
                {{ currency.code }}
              </ListboxOption>
            </ListboxOptions>
          </Listbox>
          <div>
            <label for="currency-from">From</label>
            <select id="currency-from">
              <option value="EUR">EUR</option>
              <option value="USD">USD</option>
              <option value="GBP">GBP</option>
              <option value="JPY">JPY</option>
              <option value="CAD">CAD</option>
              <option value="AUD">AUD</option>
              <option value="CHF">CHF</option>
              <option value="CNY">CNY</option>
              <option value="SEK">SEK</option>
              <option value="NZD">NZD</option>
            </select>
          </div>

          <div>
            <label for="currency-to">To</label>
            <select id="currency-to">
              <option value="USD">USD</option>
              <option value="EUR">EUR</option>
              <option value="GBP">GBP</option>
              <option value="JPY">JPY</option>
              <option value="CAD">CAD</option>
              <option value="AUD">AUD</option>
              <option value="CHF">CHF</option>
              <option value="CNY">CNY</option>
              <option value="SEK">SEK</option>
              <option value="NZD">NZD</option>
            </select>
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
          <dt>
            EUR/USD
          </dt>
        </dl>
      </section>
    </main>
  </div>
</template>

<style scoped></style>
