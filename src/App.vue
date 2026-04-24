<template>
  <div class="min-h-screen">
    <!-- Navigation Menu -->
    <nav class="fixed top-0 left-0 right-0 z-50 bg-emerald-900/95 backdrop-blur-md shadow-lg">
      <div class="max-w-6xl mx-auto px-6 py-4">
        <div class="flex items-center justify-between">
          <div class="flex items-center gap-3">
            <div class="w-10 h-10 bg-emerald-500 rounded-xl flex items-center justify-center">
              <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"/>
              </svg>
            </div>
            <span class="text-white font-bold text-xl">HilfeHerz</span>
          </div>
          <div class="hidden md:flex items-center gap-8">
            <a href="#hero" class="text-emerald-100 hover:text-white transition-colors font-medium">Start</a>
            <a href="#donate" class="text-emerald-100 hover:text-white transition-colors font-medium">Spenden</a>
            <a href="#faq" class="text-emerald-100 hover:text-white transition-colors font-medium">FAQ</a>
            <button 
              @click="openContactModal"
              class="bg-emerald-500 hover:bg-emerald-400 text-white px-5 py-2 rounded-xl font-semibold transition-all shadow-lg hover:shadow-emerald-500/30"
            >
              Kontakt
            </button>
          </div>
          <!-- Mobile menu button -->
          <button @click="mobileMenuOpen = !mobileMenuOpen" class="md:hidden text-white p-2">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path v-if="!mobileMenuOpen" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"/>
              <path v-else stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
            </svg>
          </button>
        </div>
        <!-- Mobile Menu -->
        <div v-if="mobileMenuOpen" class="md:hidden mt-4 pb-4">
          <div class="flex flex-col gap-4">
            <a href="#hero" @click="mobileMenuOpen = false" class="text-emerald-100 hover:text-white transition-colors font-medium">Start</a>
            <a href="#donate" @click="mobileMenuOpen = false" class="text-emerald-100 hover:text-white transition-colors font-medium">Spenden</a>
            <a href="#faq" @click="mobileMenuOpen = false" class="text-emerald-100 hover:text-white transition-colors font-medium">FAQ</a>
            <button 
              @click="openContactModal(); mobileMenuOpen = false"
              class="bg-emerald-500 hover:bg-emerald-400 text-white px-5 py-2 rounded-xl font-semibold transition-all"
            >
              Kontakt
            </button>
          </div>
        </div>
      </div>
    </nav>

    <HeroSection 
      :currentAmount="totalDonations" 
      :goalAmount="goalAmount"
      id="hero"
    />
    <DonationForm 
      @donation-submitted="handleDonation"
      id="donate"
    />
    <FaqSection 
      @open-contact="openContactModal"
      id="faq"
    />
    
    <!-- Footer -->
    <footer class="bg-gray-900 text-white py-8 px-6">
      <div class="max-w-4xl mx-auto text-center">
        <p class="text-gray-400 text-sm">
          &copy; 2025 Hilfsorganisation. Alle Rechte vorbehalten.
        </p>
        <p class="text-gray-500 text-xs mt-2">
          Dies ist eine fiktive Spenden-Landingpage für Demonstrationszwecke.
        </p>
      </div>
    </footer>

    <!-- Thank You Modal -->
    <ThankYouModal 
      :show="showThankYouModal"
      :amount="donatedAmount"
      @close="closeThankYouModal"
    />

    <!-- Contact Modal -->
    <ContactModal 
      :show="showContactModal"
      @close="closeContactModal"
    />
  </div>
</template>

<script>
import { ref } from 'vue'
import HeroSection from './components/HeroSection.vue'
import DonationForm from './components/DonationForm.vue'
import FaqSection from './components/FaqSection.vue'
import ThankYouModal from './components/ThankYouModal.vue'
import ContactModal from './components/ContactModal.vue'

export default {
  name: 'App',
  components: {
    HeroSection,
    DonationForm,
    FaqSection,
    ThankYouModal,
    ContactModal
  },
  setup() {
    const totalDonations = ref(4200)
    const goalAmount = ref(10000)
    const showThankYouModal = ref(false)
    const showContactModal = ref(false)
    const donatedAmount = ref(0)
    const mobileMenuOpen = ref(false)

    const handleDonation = (amount) => {
      totalDonations.value += amount
      donatedAmount.value = amount
      showThankYouModal.value = true
      
      // Scroll to top to see updated barometer
      window.scrollTo({ top: 0, behavior: 'smooth' })
    }

    const closeThankYouModal = () => {
      showThankYouModal.value = false
    }

    const openContactModal = () => {
      showContactModal.value = true
    }

    const closeContactModal = () => {
      showContactModal.value = false
    }

    return {
      totalDonations,
      goalAmount,
      showThankYouModal,
      showContactModal,
      donatedAmount,
      mobileMenuOpen,
      handleDonation,
      closeThankYouModal,
      openContactModal,
      closeContactModal
    }
  }
}
</script>
