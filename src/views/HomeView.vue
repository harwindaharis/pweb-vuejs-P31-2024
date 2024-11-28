<script lang="ts">
import BookCard from "../components/BookCard.vue";

interface BookObject {
  _id: string;
  title: string;
  author: string;
  tags: string[];
  publishedDate: string;
  initialQty: number;
  qty: number;
  coverImage: string;
  publisher: string;
  rating: {
    average: number;
  };
}

export default {
  name: "HomeView",
  data: () => ({
    booksData: [] as BookObject[],
    fetchError: false,
  }),
  async mounted() {
    try {
      const response = await fetch("http://localhost:4000/book");
      if (!response.ok) {
        throw new Error("Failed to fetch books data");
      }
      const data = await response.json();
      this.booksData = [...data.data];
    } catch (error) {
      console.error(error);
      this.fetchError = true;
    }
  },
  components: {
    BookCard,
  },
};
</script>

<template>
  <div class="bg-primary">
    <main class="container mx-auto mt-10 px-4 pb-24 ">
      
      <!-- New Welcome Section -->
      <section class="bg-secondary p-6 rounded-lg mb-10">
        <h2 class="text-3xl font-bold text-white text-center">Selamat Datang di Bookly P31</h2>
        <p class="text-white text-lg text-center mt-4">
          Platform yang menyediakan koleksi buku terbaik untuk kebutuhan bacaan Anda. Temukan buku favorit Anda atau tambahkan buku baru ke koleksi kami.
        </p>
        <div class="flex justify-center gap-4 mt-6">
          <RouterLink 
            to="/add-book"
            class="border border-white text-white px-6 py-3 rounded-lg font-bold shadow-md hover:bg-secondary hover:text-primary transition"
          >
            Add New Book
          </RouterLink>
        </div>
      </section>

      <!-- Header -->
      <h1 class="font-bold text-3xl text-center text-secondary">
        Daftar Buku Bookly P31
      </h1>

      <!-- Content Wrapper -->
      <div 
        class="grid gap-6 mt-10"
        :class="{
          'grid-cols-1 sm:grid-cols-2 lg:grid-cols-3': booksData.length && !fetchError
        }"
      >
        <!-- Book Cards -->
        <BookCard
          v-if="booksData.length"
          v-for="book in booksData"
          :key="book._id"
          :book="book"
          class="shadow-md p-4 rounded-lg bg-white hover:shadow-lg transition-shadow"
        />

        <!-- Error Message -->
        <div 
          v-else-if="fetchError"
          class="col-span-full text-center py-10"
        >
          <h1 class="font-bold text-xl text-red-600">
            Failed to load books data
          </h1>
          <p class="text-gray-600">
            Please check your internet connection or try again later.
          </p>
        </div>

        <!-- Loading Spinner -->
        <div 
          v-else 
          class="col-span-full flex justify-center items-center py-10"
        >
          <div class="animate-spin rounded-full h-12 w-12 border-t-4 border-blue-500"></div>
        </div>
      </div>
    </main>
  </div>
</template>
