<script lang="ts">
import { RouterLink } from "vue-router";
import type { PropType } from "vue";

export interface BookObject {
  _id: string;
  title: string;
  author: string;
  publishedDate: string;
  tags: string[];
  initialQty: number;
  qty: number;
  publisher: string;
  coverImage: string;
  rating: {
    average: number;
  };
}

export default {
  name: "BookCard",
  components: {
    RouterLink,
  },
  props: {
    book: {
      type: Object as PropType<BookObject>,
      required: true,
    },
  },
  methods: {
    getReadMoreLink(id: string) {
      return `/detail/${id}`;
    },
  },
};
</script>

<template>
  <div class="rounded-lg p-6 bg-white shadow-md hover:shadow-lg transition-shadow duration-300 font-poppins">
    <div class="w-full h-auto aspect-w-3 aspect-h-4 mb-4">
      <img
        :src="book.coverImage"
        alt="Cover Image"
        class="rounded-lg object-contain w-full h-full"
      />
    </div>
    <div class="flex flex-col justify-between">
      <h3 class="text-lg font-bold text-gray-800 truncate">
        {{ book.title }}
      </h3>
      <p class="text-sm text-gray-500 mt-1">
        by <span class="font-medium">{{ book.author }}</span>
      </p>

      <!-- Section for Rating -->
      <div class="flex items-center mt-2">
        <span class="text-yellow-500 flex">
          <template v-for="star in 5" :key="star">
            <svg 
              xmlns="http://www.w3.org/2000/svg" 
              width="20" 
              height="20" 
              viewBox="0 0 20 20"
              class="mr-1"
              :class="{
                'fill-current': star <= book.rating.average,
                'text-gray-300': star > book.rating.average
              }"
            >
              <path d="M10 15.27l4.15 2.18-1.08-4.75 3.67-3.42-4.84-.42L10 0 7.1 9.86l-4.84.42 3.67 3.42-1.08 4.75L10 15.27z" />
            </svg>
          </template>
        </span>
        <span class="ml-2 text-sm font-medium text-gray-600">
          {{ book.rating.average.toFixed(1) }}
        </span>
      </div>

      <hr class="my-3 border-gray-300" />
      <p class="text-sm text-gray-600">
        <span class="text-gray-800">{{ book.tags.join(", ") }}</span>
      </p>
      <p class="text-sm text-gray-700 mt-2">
        <span class="font-semibold">Quantity:</span>
        {{ book.qty }} of {{ book.initialQty }} books
      </p>
      <RouterLink
        :to="getReadMoreLink(book._id)"
        class="w-full px-4 py-2 mt-4 text-sm font-semibold text-center text-white bg-secondary rounded-lg"
      >
        Read More
      </RouterLink>
    </div>
  </div>
</template>

<style scoped>
/* Add any additional styling here */
</style>


