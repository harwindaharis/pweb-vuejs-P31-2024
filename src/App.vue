<script lang="ts">
import { defineComponent } from "vue";
import { RouterLink, RouterView } from "vue-router";

export default defineComponent({
  name: "App",
  data() {
    return {
      isMenuOpen: false,
    };
  },
  methods: {
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
    },
    closeMenu() {
      this.isMenuOpen = false;
    },
  },
  components: {
    RouterLink,
    RouterView,
  },
});
</script>

<template>
  <nav class="bg-primary p-4 font-poppins">
    <div class="container mx-auto flex justify-between items-center">
      <div class="flex gap-6">
        <img src="/images/awan.png" width="80" height="80" />
        <div class="flex items-center space-x-4">
          <h3 class="text-secondary font-bold">Bookly P31</h3>
        </div>
      </div>
      <div class="md:hidden">
        <button @click="toggleMenu" class="text-white focus:outline-none">
          <svg
            class="w-6 h-6"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M4 6h16M4 12h16m-7 6h7"
            ></path>
          </svg>
        </button>
      </div>
      <div class="hidden md:flex items-center space-x-4 text-[A5A5A5]">
        <RouterLink
          to="/"
          :class="[ 
            $route.path === '/' ? 'text-secondary font-bold' : 'text-white hover:text-secondary'
          ]"
        >Home</RouterLink>
        <RouterLink
          to="/add-book"
          :class="[
            $route.path === '/add-book' ? 'text-secondary font-bold' : 'text-white hover:text-secondary'
          ]"
        >Add New Book</RouterLink>
      </div>
    </div>
  </nav>

  <!-- Responsive Side Menu -->
  <div
    :class="{
      'translate-x-full': !isMenuOpen,
      'translate-x-0': isMenuOpen,
    }"
    class="fixed top-0 right-0 h-full w-64 bg-gray-800 text-white transform transition-transform duration-300 ease-in-out md:hidden"
  >
    <button
      @click="closeMenu"
      class="absolute top-4 right-4 text-white focus:outline-none"
    >
      <svg
        class="w-6 h-6"
        fill="none"
        stroke="currentColor"
        viewBox="0 0 24 24"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M6 18L18 6M6 6l12 12"
        ></path>
      </svg>
    </button>
    <div class="mt-16 flex flex-col items-center space-y-4">
      <RouterLink to="/" class="text-white" @click="closeMenu">Home</RouterLink>
      <RouterLink to="/add-book" class="text-white" @click="closeMenu">Add New Book</RouterLink>
    </div>
  </div>

  <!-- Main Content -->
  <RouterView />
</template>
