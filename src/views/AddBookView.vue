<script lang="ts">
import { defineComponent } from "vue";
import { format } from "date-fns";

export default defineComponent({
  name: "AddBookView",
  data() {
    return {
      title: "",
      author: "",
      publishedDate: "",
      publisher: "",
      description: "",
      coverImage: "",
      ratingAverage: 0,
      ratingCount: 0,
      tags: "",
      initialQty: 0,
      qty: 0,
      error: false,
      errorMsg: "",
    };
  },
  methods: {
    formatDate(date: string): string {
      return format(new Date(date), "dd MMMM yyyy");
    },
    async addBook() {
      const newBook = {
        title: this.title,
        author: this.author,
        publishedDate: this.formatDate(this.publishedDate),
        publisher: this.publisher,
        description: this.description,
        coverImage: this.coverImage,
        rating: {
          average: this.ratingAverage,
          count: this.ratingCount,
        },
        tags: this.tags.split(",").map((tag) => tag.trim()),
        initialQty: this.initialQty,
        qty: this.qty,
      };

      const response = await fetch("http://localhost:4000/book", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(newBook),
      });

      if (response.ok) {
        alert("Book added successfully!");
        this.error = false;
        this.$router.push("/");
      } else {
        this.error = true;
        const data = await response.json();
        if (data.message.includes("duplicate")) {
          this.errorMsg = "Book already exists!";
        } else {
          this.errorMsg = data.message;
        }
      }
    },
  },
});
</script>

<template>
  <main class="mt-10 mx-8 pb-24">
    <h1 class="font-bold text-4xl text-center text-secondary mb-2">Let's Add a Book!</h1>
    <p class="text-center text-secondary mb-8">
      Fill in the details below to add a new book to the library.
    </p>
    <form
      @submit.prevent="addBook"
      class="max-w-3xl mx-auto bg-white p-10 rounded-lg shadow-lg"
    >
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <label for="title" class="block text-black font-medium mb-2">Title</label>
          <input
            v-model="title"
            id="title"
            type="text"
            placeholder="Input Your Title Here"
            class="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-secondary focus:outline-none"
            required
          />
        </div>
        <div>
          <label for="author" class="block text-black font-medium mb-2">Author</label>
          <input
            v-model="author"
            id="author"
            type="text"
            placeholder="input Author Here"
            class="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-secondary focus:outline-none"
            required
          />
        </div>
        <div>
          <label for="publishedDate" class="block text-black font-medium mb-2">Published Date</label>
          <input
            v-model="publishedDate"
            id="publishedDate"
            type="date"
            class="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-secondary focus:outline-none"
            required
          />
        </div>
        <div>
          <label for="publisher" class="block text-black font-medium mb-2">Publisher</label>
          <input
            v-model="publisher"
            id="publisher"
            type="text"
            placeholder="input Publisher Here"
            class="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-secondary focus:outline-none"
            required
          />
        </div>
        <div class="md:col-span-2">
          <label for="description" class="block text-black font-medium mb-2">Description</label>
          <textarea
            v-model="description"
            id="description"
            placeholder="Input Your Description Here"
            class="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-secondary focus:outline-none"
            rows="4"
            required
          ></textarea>
        </div>
        <div>
          <label for="coverImage" class="block text-black font-medium mb-2">Cover Image URL</label>
          <input
            v-model="coverImage"
            id="coverImage"
            type="url"
            placeholder="https://example"
            class="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-secondary focus:outline-none"
            required
          />
        </div>
        <div>
          <label for="ratingAverage" class="block text-black font-medium mb-2">Rating Average</label>
          <input
            v-model="ratingAverage"
            id="ratingAverage"
            type="number"
            step="0.1"
            class="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-secondary focus:outline-none"
            required
          />
        </div>
        <div>
          <label for="ratingCount" class="block text-black font-medium mb-2">Rating Count</label>
          <input
            v-model="ratingCount"
            id="ratingCount"
            type="number"
            class="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-secondary focus:outline-none"
            required
          />
        </div>
        <div>
          <label for="tags" class="block text-black font-medium mb-2">Tags (comma separated)</label>
          <input
            v-model="tags"
            id="tags"
            type="text"
            placeholder="e.g. Fiction, Novel, Mystery"
            class="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-secondary focus:outline-none"
            required
          />
        </div>
        <div>
          <label for="initialQty" class="block text-black font-medium mb-2">Initial Quantity</label>
          <input
            v-model="initialQty"
            id="initialQty"
            type="number"
            class="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-secondary focus:outline-none"
            required
          />
        </div>
        <div>
          <label for="qty" class="block text-black font-medium mb-2">Quantity</label>
          <input
            v-model="qty"
            id="qty"
            type="number"
            class="w-full px-4 py-3 border rounded-lg focus:ring-2 focus:ring-secondary focus:outline-none"
            required
          />
        </div>
      </div>
      <p
        class="font-medium text-red-500 mt-6 text-center"
        v-show="error"
      >
        {{ errorMsg }}
      </p>
      <div class="flex justify-center mt-8">
        <button
          type="submit"
          class="px-6 py-3 bg-secondary text-white rounded-lg text-bold shadow-lg transition-transform"
        >
          Add Book
        </button>
      </div>
    </form>
  </main>
</template>

