<script lang="ts">
import { defineComponent } from "vue";

interface Rating {
  average: number;
  count: number;
}

interface BookDetail {
  rating: Rating;
  _id: string;
  title: string;
  author: string;
  publishedDate: string;
  publisher: string;
  description: string;
  coverImage: string;
  tags: string[];
  initialQty: number;
  qty: number;
  createdAt: string;
  updatedAt: string;
  __v: number;
}

export default defineComponent({
  name: "DetailBookView",
  data: () => ({
    bookDetail: {} as BookDetail,
    fetchError: false,
  }),
  async mounted() {
    try {
      const response = await fetch(
        `http://localhost:4000/book/${this.$route.params.id}`
      );
      if (!response.ok) {
        throw new Error("Failed to fetch book data");
      }
      const data = await response.json();
      this.bookDetail = { ...data.data };
    } catch (error) {
      console.error(error);
      this.fetchError = true;
    }
  },
  methods: {
    async deleteBook() {
      const response = await fetch(
        `http://localhost:4000/book/${this.$route.params.id}`,
        {
          method: "DELETE",
        }
      );
      const data = await response.json();
      console.log(data);
      alert("Book has been removed!");
      this.$router.push("/");
    },
  },
  computed: {
    starRating(): string {
      const stars = Math.floor(this.bookDetail.rating.average);
      return "⭐".repeat(stars);
    },
  },
});
</script>

<template>
  <main class="mt-14 mx-8 pb-14 font-poppins">
    <!-- Back to Home Button -->
    <RouterLink
      to="/"
      class="flex items-center justify-center gap-4 py-3 w-24 lg:w-32 text-white bg-secondary font-semibold rounded-xl shadow-lg transition-transform"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 20 20"
        stroke-width="2"
        stroke="currentColor"
        class="w-3 h-3"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M15 19l-7-7 7-7"
        />
      </svg>
      Back
    </RouterLink>

    <!-- Error Message -->
    <div v-if="fetchError" class="mt-16 text-center">
      <h1 class="text-3xl font-extrabold text-gray-800">Failed to load book data</h1>
      <p class="text-lg text-gray-600 mt-4">Please check your connection or try again later.</p>
    </div>

    <!-- Book Details -->
    <div v-else-if="bookDetail.title" class="mt-12">
      <div class="flex flex-col lg:flex-row lg:ml-24 gap-12">
        <!-- Book Cover -->
        <div class="w-full lg:w-[500px] flex-shrink-0">
          <img
            :src="bookDetail.coverImage"
            class="rounded-xl w-full shadow-md"
            alt="Book Cover"
          />
        </div>

        <!-- Book Information -->
        <div class="flex-1">
          <h1 class="text-3xl font-extrabold text-gray-900">
            {{ bookDetail.title }}
          </h1>
          <p class="text-lg font-semibold text-secondary mt-2">
            by {{ bookDetail.author }}
          </p>

          <!-- Rating -->
          <div class="flex items-center gap-2 mt-4">
            <span class="text-lg font-bold text-secondary">
              {{ bookDetail.rating.average }}
            </span>
            <span class="text-xl">{{ starRating }}</span>
            <span class="text-gray-600 text-sm">({{ bookDetail.rating.count }} reviews)</span>
          </div>

          <hr class="border-gray-300 my-6" />

          <!-- Description -->
          <div class="mb-4">
            <h3 class="text-lg font-semibold text-gray-800">Description:</h3>
            <p class="text-gray-700 mt-2">{{ bookDetail.description }}</p>
          </div>

          <!-- Additional Details -->
          <ul class="text-gray-700 space-y-2">
            <li>
              <span class="font-semibold text-gray-900">Published:</span> {{ bookDetail.publishedDate }}
              by {{ bookDetail.publisher }}
            </li>
            <li>
              <span class="font-semibold text-gray-900">Category:</span>
              {{ bookDetail.tags.join(", ") }}
            </li>
            <li>
              <span class="font-semibold text-gray-900">Stock:</span>
              {{ bookDetail.qty }} of {{ bookDetail.initialQty }} books
            </li>
          </ul>
        </div>
      </div>

      <!-- Remove Book Button -->
      <div class="mt-12 flex justify-center lg:justify-start lg:ml-24">
        <button
          @click="deleteBook"
          class="px-6 py-3 w-full lg:w-full bg-secondary text-white font-semibold rounded-xl shadow-lg transition-transform hover:scale-105"
        >
          Remove
        </button>
      </div>
    </div>

    <!-- Loading State -->
    <div v-else class="mt-16 text-center">
      <h1 class="text-3xl font-extrabold text-gray-800">Loading...</h1>
      <p class="text-lg text-gray-600 mt-4">Please wait while we fetch the data.</p>
    </div>
  </main>
</template>

