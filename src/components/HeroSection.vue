<template>
  <section class="relative overflow-hidden bg-gradient-to-br from-emerald-600 via-teal-600 to-cyan-600 text-white py-16 md:py-20 lg:py-24 px-6 pt-24">
    <!-- Decorative background elements -->
    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <div class="absolute top-0 left-0 w-96 h-96 bg-teal-400/20 rounded-full blur-3xl -translate-x-1/2 -translate-y-1/2"></div>
      <div class="absolute bottom-0 right-0 w-[500px] h-[500px] bg-cyan-400/20 rounded-full blur-3xl translate-x-1/2 translate-y-1/2"></div>
      <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[600px] h-[600px] bg-white/10 rounded-full blur-3xl"></div>
    </div>

    <div class="max-w-4xl mx-auto relative z-10">
      <!-- Hero Content -->
      <div class="text-center mb-12 md:mb-16 animate-slide-up">
        <div class="inline-flex items-center justify-center w-20 h-20 mb-6 bg-white/20 backdrop-blur-sm rounded-2xl shadow-2xl">
          <svg class="w-10 h-10 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"/>
          </svg>
        </div>
        <h1 class="text-4xl md:text-5xl lg:text-6xl font-bold mb-4 md:mb-6 leading-tight drop-shadow-lg">
          Gemeinsam Hoffnung schenken
        </h1>
        <p class="text-lg md:text-xl text-white/95 max-w-2xl mx-auto leading-relaxed font-medium">
          Ihre Spende macht einen Unterschied. Helfen Sie uns, Familien in Not zu unterstützen 
          und eine bessere Zukunft für alle zu schaffen.
        </p>
      </div>

      <!-- Donation Barometer Card -->
      <div class="bg-white/95 backdrop-blur-xl rounded-3xl shadow-2xl shadow-emerald-900/20 p-6 md:p-8 lg:p-10 text-gray-900 animate-fade-in border-2 border-white/60">
        <div class="mb-6 md:mb-8">
          <div class="flex items-center gap-3 mb-3">
            <div class="w-10 h-10 bg-gradient-to-br from-emerald-500 to-cyan-600 rounded-xl flex items-center justify-center">
              <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"/>
              </svg>
            </div>
            <h2 class="text-2xl md:text-3xl font-bold bg-gradient-to-r from-emerald-900 to-teal-900 bg-clip-text text-transparent">
              Spendenbarometer
            </h2>
          </div>
          <div class="flex items-baseline gap-2 flex-wrap">
            <span class="text-3xl md:text-4xl lg:text-5xl font-bold bg-gradient-to-r from-emerald-600 to-cyan-600 bg-clip-text text-transparent">
              {{ formatCurrency(currentAmount) }}
            </span>
            <span class="text-xl md:text-2xl text-gray-500 font-semibold">
              von {{ formatCurrency(goalAmount) }}
            </span>
          </div>
        </div>
        
        <!-- Progress Bar -->
        <div class="relative h-10 md:h-12 bg-gradient-to-r from-emerald-100 to-cyan-100 rounded-full overflow-hidden mb-6 md:mb-8 shadow-inner">
          <div 
            class="absolute inset-y-0 left-0 rounded-full transition-all duration-700 ease-out flex items-center justify-end px-4"
            :class="isGoalReached ? 'bg-gradient-to-r from-green-500 to-emerald-600' : 'bg-gradient-to-r from-emerald-600 via-teal-600 to-cyan-600'"
            :style="{ width: progressPercentage + '%' }"
          >
            <!-- Shimmer effect -->
            <div class="absolute inset-0 overflow-hidden">
              <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white/30 to-transparent animate-shimmer"></div>
            </div>
            
            <span v-if="progressPercentage > 10" class="relative z-10 text-white font-bold text-sm md:text-base drop-shadow">
              {{ progressPercentage }}%
            </span>
          </div>
        </div>
        
        <!-- Stats Grid -->
        <div class="grid grid-cols-2 gap-4 md:gap-6">
          <div class="bg-gradient-to-br from-emerald-50 to-teal-50 rounded-xl p-4 md:p-6 text-center border-2 border-emerald-100/60">
            <div class="text-2xl md:text-3xl lg:text-4xl font-bold bg-gradient-to-r from-emerald-600 to-teal-600 bg-clip-text text-transparent mb-1">
              {{ progressPercentage }}%
            </div>
            <div class="text-sm md:text-base text-gray-700 font-bold">
              Erreicht
            </div>
          </div>
          <div class="bg-gradient-to-br from-cyan-50 to-blue-50 rounded-xl p-4 md:p-6 text-center border-2 border-cyan-100/60">
            <div class="text-2xl md:text-3xl lg:text-4xl font-bold bg-gradient-to-r from-cyan-600 to-blue-600 bg-clip-text text-transparent mb-1">
              {{ formatCurrency(remainingAmount) }}
            </div>
            <div class="text-sm md:text-base text-gray-700 font-bold">
              Noch benötigt
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { computed } from 'vue'

export default {
  name: 'HeroSection',
  props: {
    currentAmount: {
      type: Number,
      required: true
    },
    goalAmount: {
      type: Number,
      required: true
    }
  },
  setup(props) {
    const progressPercentage = computed(() => {
      const percentage = (props.currentAmount / props.goalAmount) * 100
      return Math.min(Math.round(percentage), 100)
    })

    const remainingAmount = computed(() => {
      return Math.max(props.goalAmount - props.currentAmount, 0)
    })

    const isGoalReached = computed(() => {
      return props.currentAmount >= props.goalAmount
    })

    const formatCurrency = (amount) => {
      return new Intl.NumberFormat('de-DE', {
        style: 'currency',
        currency: 'EUR',
        minimumFractionDigits: 0,
        maximumFractionDigits: 0
      }).format(amount)
    }

    return {
      progressPercentage,
      remainingAmount,
      isGoalReached,
      formatCurrency
    }
  }
}
</script>

<style scoped>
/* All styling is now handled by Tailwind utility classes */
</style>
