<template>
  <div>
    <div class="font-bold" :class="[props.color]">{{ props.title }}</div>
    <div class="text-2xl font-extrabold text-black dark:text-white mb-2">
      <USkeleton class="h-8 w-full" v-if="loading" />
      <div class="text-white" v-else>{{ currency }}</div>
    </div>
    <div>
      <USkeleton class="h-6 w-full" v-if="loading" />
      <div v-else class="flex space=x=1 items-center text-sm">
        <UIcon :name="icon" class="w-6 h-6" :class="{'green': trendingUp, 'red': !trendingUp}" />
        <!-- <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" :stroke="[props.color]" class="size-6">
          <path stroke-linecap="round" stroke-linejoin="round" d="M2.25 18 9 11.25l4.306 4.306a11.95 11.95 0 0 1 5.814-5.518l2.74-1.22m0 0-5.94-2.281m5.94 2.28-2.28 5.941" />
        </svg> -->
        <div class="text-gray-500 dark:text-gray-400">
          {{ percentageTrend }} vs last period
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
interface Props{
  title: String,
  amount: number,
  lastAmount: number,
  color: String,
  loading: Boolean
}
const props = defineProps<Props>()

const trendingUp = computed(
  () => props.amount >= props.lastAmount
)
const icon = computed(
  () => trendingUp.value ? 'i-heroicons-arrow-trending-up' : 'i-heroicons-arrow-trending-down'
)
const {currency} = useCurrency(props.amount)
const percentageTrend = computed(() => {
  if (props.amount === 0 || props.lastAmount === 0) return '∞%'
  const bigger = Math.max(props.amount, props.lastAmount)
  const lower = Math.min(props.amount, props.lastAmount)
  const ratio = ((bigger - lower) / lower) * 100
  // console.log(bigger, lower, ratio, Math.ceil(ratio))
  return `${Math.ceil(ratio)}%`
})
</script>

<style lang="css" scoped>
.green{
  @apply text-green-600 dark:text-green-400
}

.red{
  @apply text-red-600 dark:text-red-400
}
</style>