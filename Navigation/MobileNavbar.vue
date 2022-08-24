<template>
  <nav class="md:hidden bg-white shadow">
    <div class="max-w-7xl md:mx-auto px-4 sm:px-6 lg:px-8">

      <div class="flex flex-row justify-between h-16">
        <div class="flex mt-2 md:mt-0">

          <div class="flex-shrink-0 flex items-center">
            <nuxt-link to="/">
              <img alt="Morph Logo" class="h-7 w-28 md:h-9 md:w-32" width="132" height="39" src="/logo.svg">
            </nuxt-link>
          </div>

        </div>

        <div class="flex items-center space-x-2">

          <div class="flex items-center space-x-1 sm:space-x-10">

            <slot name="actions"/>

            <div v-if="!$config.beta" class="relative flex justify-end md:ml-4 md:flex-shrink-0 items-center space-x-1">

              <language-switcher/>

              <cart-button @click="cartSlideOver.open()"/>

              <!-- Notifications Dropdown -->
              <generic-notifications-dropdown v-if="$auth.loggedIn"/>

              <command-trigger-button/>

              <div class="flex flex-col space-y-4">
                <div v-if="!$auth.loggedIn" class="flex-shrink-0 flex items-center space-x-1">
                  <nuxt-link
                    v-if="!$config.beta"
                    to="/auth/login"
                    class="relative inline-flex items-center text-sm font-semibold rounded-md text-slate-700  hover:text-slate-800 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-slate-500"
                  >
                    <user-circle-icon
                      v-if="!$config.beta"
                      class="w-5 h-5 text-gray-500"/>

                    <span class="hidden md:block">{{ $t('Log in') }}</span>
                  </nuxt-link>
                </div>
              </div>
            </div>

            <div v-if="!$config.beta" class="flex items-center md:hidden">
              <!-- Mobile menu button -->
              <button aria-controls="mobile-menu"
                      aria-expanded="false"
                      @click="mobileMenu.open()"
                      class="inline-flex items-center justify-center rounded-md text-slate-400 hover:text-slate-500 hover:bg-slate-100 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-indigo-500"
                      type="button">
                <span class="sr-only">{{ $t('Open main menu') }}</span>
                <!--
                  Icon when menu is closed.

                  Heroicon name: outline/menu

                  Menu open: "hidden", Menu closed: "block"
                -->
                <svg aria-hidden="true" class="block h-5 w-5" fill="none" stroke="currentColor"
                     viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                  <path d="M4 6h16M4 12h16M4 18h16" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"/>
                </svg>
                <!--
                  Icon when menu is open.

                  Heroicon name: outline/x

                  Menu open: "block", Menu closed: "hidden"
                -->
                <svg aria-hidden="true" class="hidden h-6 w-6" fill="none" stroke="currentColor"
                     viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                  <path d="M6 18L18 6M6 6l12 12" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"/>
                </svg>
              </button>
            </div>

          </div>
        </div>

      </div>
    </div>

    <slide-over v-if="!$config.beta" ref="mobile-menu" class="md:hidden">
      <template #title>
        <nuxt-link to="/">
          <img alt="Morph" class="h-9 w-32 w-auto" width="132" height="39" src="/logo.svg">
        </nuxt-link>
        <p class="text-sm text-gray-500">A flexible way to code.</p>
      </template>

      <collections-tree-menu/>
    </slide-over>

    <cart-slider-over ref="cart-slide-over"/>

  </nav>
</template>

<script>
import NavItem from "~/ui/NavItem";
import UiButton from "~/ui/Button";
import UiDropdown from '~/ui/Dropdown';
import UiDropdownLink from '~/ui/DropdownLink';
import UiDialogModal from '~/ui/DialogModal';
import NotificationsDropdown from '~/components/Notifications/Dropdown';
import {UserCircleIcon} from "@vue-hero-icons/solid"
import {ShoppingCartIcon} from "@vue-hero-icons/outline"

import FloatingCart from "@/components/Shop/FloatingCart";
import GenericNotificationsDropdown from "~/components/Generic/NotificationsDropdown"
import LanguageSwitcher from "~/components/Utils/LanguageSwitcher";
import SlideOver from "~/ui/SlideOver";
import CollectionsTreeMenu from "~/components/Collections/TreeMenu";
import TextField from "~/ui/Fields/Text";
import CommandTriggerButton from "~/components/CommandPalette/CommandTriggerButton";
import CartSliderOver from "~/components/Shop/CartSliderOver";
import CartButton from "~/components/Shop/CartButton";

export default {
  components: {
    CartButton,
    CartSliderOver,
    CommandTriggerButton,
    TextField,
    CollectionsTreeMenu,
    SlideOver,
    LanguageSwitcher,
    FloatingCart,
    NavItem,
    UiButton,
    UiDropdown,
    UiDropdownLink,
    UiDialogModal,
    NotificationsDropdown,
    UserCircleIcon,
    GenericNotificationsDropdown,
    ShoppingCartIcon,
  },
  data() {
    return {
      show: false,
    }
  },

  computed: {
    mobileMenu() {
      return this.$refs['mobile-menu'];
    },

    cartSlideOver() {
      return this.$refs['cart-slide-over'];
    },
  },

  created() {
    this.$nuxt.$on('swipe', (direction) => {
      if (direction === 'left') {
        try {
          this.mobileMenu.open();
        } catch (e) {

        }
      }
    })
  },

  methods: {
    async logout() {
      await this.$auth.logout();
    },
  },
}
</script>
