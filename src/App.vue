<script setup lang="ts">
import { computed, ref, watch, onMounted } from 'vue'
import CurrencyInput from './components/CurrencyInput.vue'
import CurrencySelect from './components/CurrencySelect.vue'

const fromCurrency = ref('USD')
const sendAmount = ref('1000')
const toCurrency = ref('EUR')
const receiveAmount = ref('900')

const currentRate = ref(1)
const isLoading = ref(false)

const getRates = async (base: string, target: string) => {
  if (base === target) {
    currentRate.value = 1
    return
  }

  try {
    isLoading.value = false
    const res = await fetch(`https://api.frankfurter.dev/v2/rate/${base}/${target}`)
    if (!res.ok) throw new Error('Failed to fetch rates')
    const data = await res.json()
    currentRate.value = data.rate
  } catch (error) {
    console.error(error)
  } finally {
    isLoading.value = true
  }
}

const calculatedReceive = computed(() => {
  const amount = parseFloat(sendAmount.value) || 0
  return (amount * currentRate.value).toFixed(2)
})

const updateConversion = () => {
  const amount = parseFloat(sendAmount.value) || 0
  receiveAmount.value = (amount * currentRate.value).toFixed(2)
}

watch([fromCurrency, toCurrency], async ([newBase, newTarget]) => {
  await getRates(newBase, newTarget)
  updateConversion()
})

watch(sendAmount, () => {
  updateConversion()
})

onMounted(async () => {
  await getRates(fromCurrency.value, toCurrency.value)
  updateConversion()
})

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

const swapCurrencies = () => {
  const temp = fromCurrency.value
  fromCurrency.value = toCurrency.value
  toCurrency.value = temp
}
</script>

<template>
  <div class="main-container bg-neutral-900 font-mono">
    <header class="p-4">
      <img src="@/assets/images/logo.svg" alt="Foreign Exchange Checker Logo" />
    </header>

    <main class="p-4">
      <section aria-labelledby="converter-heading">
        <h2 id="converter-heading" class="font-semibold text-neutral-50 text-xl">CHECK THE RATE</h2>

        <form action="/converter" method="post" class="bg-neutral-700 rounded-2xl pb-4">
          <div class="p-4 flex flex-col gap-4 mt-4">
            <div
              class="bg-neutral-600 border-neutral-500 border p-4 rounded-2xl flex flex-col gap-5"
            >
              <p class="text-neutral-100">SEND</p>
              <div class="flex justify-between">
                <CurrencyInput v-model="sendAmount" class="text-neutral-50" />
                <CurrencySelect v-model="fromCurrency" :currencies="currencies" />
              </div>
            </div>

            <div
              class="bg-neutral-600 border-neutral-500 border mx-auto flex items-center justify-center w-12 h-12 rounded-xl"
            >
              <button type="button" @click="swapCurrencies">
                <img src="@/assets/images/icon-exchange-vertical.svg" alt="Swap currencies" />
              </button>
            </div>

            <div
              class="bg-neutral-600 border-neutral-500 border p-4 rounded-2xl flex flex-col gap-5"
            >
              <p class="text-neutral-100">RECEIVE</p>
              <div class="flex justify-between">
                <CurrencyInput v-model="receiveAmount" class="text-lime-500" readonly />
                <CurrencySelect v-model="toCurrency" :currencies="currencies" />
              </div>
            </div>
          </div>

          <svg class="w-full h-[1px] my-2" xmlns="http://www.w3.org/2000/svg">
            <!-- 8 is the dash length, 6 is the space between dashes -->
            <line
              x1="0"
              y1="0"
              x2="100%"
              y2="0"
              stroke="currentColor"
              stroke-dasharray="8 6"
              class="text-neutral-400"
            />
          </svg>

          <div>
            <p class="text-center text-neutral-50 text-[10px]">1 {{ fromCurrency }} = {{ currentRate.toFixed(4) }} {{ toCurrency }}</p>
            <div class="flex items-center justify-center gap-2 mt-4">
              <button
                type="button"
                class="bg-lime-500 py-2 px-3 rounded-xl flex items-center gap-2"
              >
                <img
                  src="@/assets/images/icon-star-black.svg"
                  alt="Add to favorites"
                  class="w-4 h-4"
                />FAVORITED
              </button>
              <button
                type="button"
                class="border border-lime-500 text-neutral-50 py-2 px-3 rounded-xl"
              >
                LOG CONVERSION
              </button>
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
