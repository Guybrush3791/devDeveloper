<template>
<nav class="backdrop-filter backdrop-blur sticky top-0 z-40 w-full">
  <div class="max-w-7xl border-l border-r border-dashed border-gray-700 mx-auto px-2 sm:px-4 lg:px-8">
    <div class="relative flex items-center justify-between h-16">
      <div class="flex items-center px-2 lg:px-0">
        <nuxt-link :to="localePath('/')" class="flex-shrink-0">
          <TheLogo class="text-3xl w-auto" />
        </nuxt-link>
        <div class="hidden lg:block lg:ml-6">
          <div class="flex space-x-4">
            <nuxt-link exact :to="localePath('/')" class="active text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-bold">{{ $t('nav.home')}}</nuxt-link>
            <nuxt-link v-show="$config.blog.enabled" :to="localePath('/blog')" class="active text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-bold">{{ $t('nav.blog')}}</nuxt-link>
            <nuxt-link v-show="$config.projects.enabled" :to="localePath('/projects')" class="active text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-bold">{{ $t('nav.projects')}}</nuxt-link>
            <nuxt-link v-show="$config.uses.enabled" :to="localePath('/uses')" class="active text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-bold">{{ $t('nav.uses')}}</nuxt-link>
            <nuxt-link v-show="$config.resume.enabled" :to="localePath('/resume')" class="active text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-bold">{{ $t('nav.resume')}}</nuxt-link>
            
          </div>
        </div>
      </div>
      <div class="flex lg:hidden">
        <div class="mr-2 flex items-center">
          <button @click="mobileMenuOpen = true" type="button" class="inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:bg-gray-700 focus:outline-none focus:bg-gray-700 transition duration-150 ease-in-out" id="main-menu" aria-label="Main menu" aria-haspopup="true">
          <svg class="h-6 w-6" stroke="currentColor" fill="none" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
          </svg>
          </button>
        </div>
      </div>
      <div class="hidden lg:flex flex-row space-x-2 items-center justify-center">
        <div v-if="$config.laguageSwitcher.enabled">
          <nuxt-link v-for="locale in availableLocales" :key="locale.code" :to="switchLocalePath(locale.code)" class="text-gray-300 hover:text-white" >{{ locale.name }}</nuxt-link>
        </div>
        <div v-if="$config.firebase.enabled">
          <div v-if="!user" @click="signInUser" class="active cursor-pointer text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-bold">{{ $t('nav.signIn')}}</div>
          <div v-else @click="signOutUser" class="active cursor-pointer text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-bold">{{ $t('nav.signOut')}}</div>
        </div>
        <a v-if="$config.blog.enabled" class="text-gray-300 hover:text-white" href="/feed.xml" target="_blank">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
            <path d="M5 3a1 1 0 000 2c5.523 0 10 4.477 10 10a1 1 0 102 0C17 8.373 11.627 3 5 3z" />
            <path d="M4 9a1 1 0 011-1 7 7 0 017 7 1 1 0 11-2 0 5 5 0 00-5-5 1 1 0 01-1-1zM3 15a2 2 0 114 0 2 2 0 01-4 0z" />
          </svg>
        </a>
      </div>
      <!-- <a :href="$config.buyMeACoffee.url" target="_blank" rel="noreferrer" class="active text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-bold">{{ $t( 'nav.login' )}}</a> -->
      <!-- text-gray-300 hover:bg-gray-700 hover:text-white text-sm font-bold -->
      <!-- hs-dropdown-toggle py-3 px-4 inline-flex justify-center items-center gap-2 rounded-md border font-medium bg-white text-gray-700 shadow-sm align-middle hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-white focus:ring-blue-600 transition-all text-sm dark:bg-slate-900 dark:hover:bg-slate-800 dark:border-gray-700 dark:text-gray-400 dark:hover:text-white dark:focus:ring-offset-gray-800 -->
      <div class="hs-dropdown relative inline-flex [--trigger:hover]">
  <button id="hs-dropdown-hover-event" type="button" class="hs-dropdown-toggle py-3 px-4 inline-flex justify-center items-center gap-2 rounded-md border font-medium bg-white text-gray-700 shadow-sm align-middle hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-white focus:ring-blue-600 transition-all text-sm dark:bg-slate-900 dark:hover:bg-slate-800 dark:border-gray-700 dark:text-gray-400 dark:hover:text-white dark:focus:ring-offset-gray-800">
    Actions
    <svg class="hs-dropdown-open:rotate-180 w-2.5 h-2.5 text-gray-600" width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M2 5L8.16086 10.6869C8.35239 10.8637 8.64761 10.8637 8.83914 10.6869L15 5" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
    </svg>
  </button>

  <div class="hs-dropdown-menu transition-[opacity,margin] duration hs-dropdown-open:opacity-100 opacity-0 hidden mt-2 min-w-[15rem] bg-white shadow-md rounded-lg p-2 mt-2 dark:bg-gray-800 dark:border dark:border-gray-700 dark:divide-gray-700 after:h-4 after:absolute after:-bottom-4 after:left-0 after:w-full before:h-4 before:absolute before:-top-4 before:left-0 before:w-full" aria-labelledby="hs-dropdown-hover-event">
    <a class="flex items-center gap-x-3.5 py-2 px-3 rounded-md text-sm text-gray-800 hover:bg-gray-100 focus:ring-2 focus:ring-blue-500 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-gray-300" href="#">
      Newsletter
    </a>
    <a class="flex items-center gap-x-3.5 py-2 px-3 rounded-md text-sm text-gray-800 hover:bg-gray-100 focus:ring-2 focus:ring-blue-500 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-gray-300" href="#">
      Purchases
    </a>
    <a class="flex items-center gap-x-3.5 py-2 px-3 rounded-md text-sm text-gray-800 hover:bg-gray-100 focus:ring-2 focus:ring-blue-500 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-gray-300" href="#">
      Downloads
    </a>
    <a class="flex items-center gap-x-3.5 py-2 px-3 rounded-md text-sm text-gray-800 hover:bg-gray-100 focus:ring-2 focus:ring-blue-500 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-gray-300" href="#">
      Team Account
    </a>
  </div>
</div>
    </div>
  </div>
  <transition
    enter-active-class="duration-150 ease-out"
    enter-class="opacity-0 scale-95"
    enter-to-class="opacity-100 scale-100"
    leave-active-class="duration-100 ease-in"
    leave-class="opacity-100 scale-100"
    leave-to-class="opacity-0 scale-95"
  >
    <div
      v-show="mobileMenuOpen"
      class=" absolute top-0 inset-x-0 p-2 transition transform origin-top-right lg:hidden">
      <div class="rounded-lg bg-gray-800">
        <div class="bg-gray-900 rounded-lg" role="menu" aria-orientation="vertical" aria-labelledby="main-menu">
          <div class="px-5 pt-4 flex items-center justify-between">
            <div @click="mobileMenuOpen = false">
              <nuxt-link exact :to="localePath('/')">
                <TheLogo class="text-3xl w-auto"  />
              </nuxt-link>
            </div>
              <div class="-mr-2">
              <button @click="mobileMenuOpen = false" type="button" class="inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:bg-gray-100 focus:outline-none focus:bg-gray-100 focus:text-gray-500 transition duration-150 ease-in-out" aria-label="Close menu">
                <svg class="h-6 w-6" stroke="currentColor" fill="none" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                </svg>
              </button>
            </div>
          </div>
          <div @click="mobileMenuOpen = false" class="flex flex-col space-y-1 px-2 pt-2 pb-3">
            <nuxt-link exact :to="localePath('/')" class="flex px-3 py-2 rounded-md text-base font-medium text-gray-200 hover:text-gray-100 hover:bg-gray-600 focus:outline-none focus:text-gray-100 focus:bg-gray-500 transition duration-150 ease-in-out" role="menuitem">{{ $t('nav.home')}}</nuxt-link>
            <nuxt-link v-show="$config.blog.enabled" :to="localePath('/blog')" class="flex px-3 py-2 rounded-md text-base font-medium text-gray-200 hover:text-gray-100 hover:bg-gray-600 focus:outline-none focus:text-gray-100 focus:bg-gray-500 transition duration-150 ease-in-out" role="menuitem">{{ $t('nav.blog')}}</nuxt-link>
            <nuxt-link v-show="$config.projects.enabled" :to="localePath('/projects')" class="flex px-3 py-2 rounded-md text-base font-medium text-gray-200 hover:text-gray-100 hover:bg-gray-600 focus:outline-none focus:text-gray-100 focus:bg-gray-500 transition duration-150 ease-in-out" role="menuitem">{{ $t('nav.projects')}}</nuxt-link>
            <nuxt-link v-show="$config.uses.enabled" :to="localePath('/uses')" class="flex px-3 py-2 rounded-md text-base font-medium text-gray-200 hover:text-gray-100 hover:bg-gray-600 focus:outline-none focus:text-gray-100 focus:bg-gray-500 transition duration-150 ease-in-out" role="menuitem">{{ $t('nav.uses')}}</nuxt-link>
            <nuxt-link v-show="$config.resume.enabled" :to="localePath('/resume')" class="flex px-3 py-2 rounded-md text-base font-medium text-gray-200 hover:text-gray-100 hover:bg-gray-600 focus:outline-none focus:text-gray-100 focus:bg-gray-500 transition duration-150 ease-in-out" role="menuitem">{{ $t('nav.resume')}}</nuxt-link>
            <a v-show="$config.buyMeACoffee.enabled" :href="$config.buyMeACoffee.url" target="_blank" rel="noreferrer" class="flex px-3 py-2 rounded-md text-base font-medium text-gray-200 hover:text-gray-100 hover:bg-gray-600 focus:outline-none focus:text-gray-100 focus:bg-gray-500 transition duration-150 ease-in-out" role="menuitem">{{ $t('nav.buyMeACoffee')}}</a>
          </div>
          <div v-if="$config.firebase.enabled">
              <div v-if="!user" @click="signInUser" class="block w-full px-5 py-3 text-center font-medium text-gray-200 bg-indigo-700 hover:bg-indigo-600 hover:text-gray-200 focus:outline-none focus:bg-indigo-600 focus:text-gray-100 transition duration-150 ease-in-out">{{ $t('nav.signIn') }}</div>
              <div v-else @click="signOutUser" class="block w-full px-5 py-3 text-center font-medium text-gray-200 bg-indigo-700 hover:bg-indigo-600 hover:text-gray-200 focus:outline-none focus:bg-indigo-600 focus:text-gray-100 transition duration-150 ease-in-out">{{ $t('nav.signOut') }}</div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</nav>
</template>

<script>
import TheLogo from "~/components/logos/TheLogo";
export default {
  components: {TheLogo},
  computed: {
    user() {
      return this.$store.state.user
    },
    availableLocales () {
      return this.$i18n.locales.filter(i => i.code !== this.$i18n.locale)
    }
  },
  data() {
    return {
      mobileMenuOpen: false,
      toastOptions: { duration: 3000, theme: 'bubble' }
    }
  },
  methods: {
    async signInUser() {
      this.mobileMenuOpen = false
      try {
        const user = await this.$store.dispatch('signInUserWithGoogle')
        this.$toast.success(`welcome ${user.displayName.toLowerCase()} 🙌`, this.toastOptions)
      } catch (e) {
        console.error(e)
        this.$toast.error(e.toString(), this.toastOptions)
      }
    },
    signOutUser() {
      this.mobileMenuOpen = false
      this.$store.dispatch('signOut')
      this.$toast.show('see you next time 👋', this.toastOptions)
    }
  },
}
</script>

<style scoped>
.backdrop-filter {
  backdrop-filter: blur(15px);
}

.nuxt-link-active.active {
  @apply bg-gray-900 text-white;
}

.nuxt-link-active.active-mobile {
  @apply bg-gray-900 text-white;
}
</style>
