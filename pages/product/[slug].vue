<template>
  <div class="container mx-auto p-6 space-y-8">
    <!-- Navigation -->
    <nav class="flex items-center">
      <router-link
        to="/"
        class="text-blue-600 hover:text-blue-800 font-medium flex items-center space-x-2"
      >
        <span>←</span>
        <span>Lista de produtos</span>
      </router-link>
    </nav>

    <PaymentInfo />

    <!-- Product Details -->
    <ContentRenderer :value="product">
      <div class="space-y-6">
        <div>
          <h2 class="text-3xl font-extrabold text-gray-800">{{ product.title }}</h2>
          <p class="text-xl text-gray-500 mt-2">{{ formatCurrency(product.price) }}</p>
        </div>

        <ContentRendererMarkdown :value="product" class="prose" />
      </div>

      <!-- Image Gallery -->
      <div>
        <h3 class="text-2xl font-semibold text-gray-800 mb-4">Galeria de imagens</h3>

        <!-- Image Navigation Menu -->
        <div class="flex justify-center space-x-2 mb-4">
          <button
            v-for="(image, index) in product.images"
            :key="index"
            @click="goToImage(index)"
            :class="[
              'px-3 py-2 rounded-full transition font-medium text-sm',
              currentImageIndex === index
                ? 'bg-blue-600 text-white'
                : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
            ]"
          >
            {{ index + 1 }}
          </button>
        </div>

        <!-- Image Display -->
        <div class="relative image-gallery">
          <img
            :src="currentImage"
            alt="Imagem do Produto"
            class="rounded-lg shadow-md"
          />

          <!-- Previous Button -->
          <button
            @click="prevImage"
            class="absolute left-2 top-1/2 transform -translate-y-1/2 px-3 py-2 bg-gray-800 text-white rounded-full hover:bg-gray-900 transition"
          >
            ←
          </button>

          <!-- Next Button -->
          <button
            @click="nextImage"
            class="absolute right-2 top-1/2 transform -translate-y-1/2 px-3 py-2 bg-gray-800 text-white rounded-full hover:bg-gray-900 transition"
          >
            →
          </button>
        </div>
      </div>
    </ContentRenderer>
  </div>
</template>

<script setup async>
import { useRoute } from 'vue-router';
import { ref, computed } from 'vue';
import PaymentInfo from '~/components/PaymentInfo.vue';
import customHead from '~/components/CustomHead.vue';

customHead();

const route = useRoute();
const product = ref(null);
const currentImageIndex = ref(0);
const slug = route.params.slug;

const { data: fetchedProduct } = await useAsyncData(slug, () => queryContent('products').where({ slug: { $eq: slug } }).findOne());
product.value = fetchedProduct.value;

const prevImage = () => {
  if (currentImageIndex.value > 0) {
    currentImageIndex.value--;
  } else {
    currentImageIndex.value = product.value.images.length - 1;
  }
};

const nextImage = () => {
  if (currentImageIndex.value < product.value.images.length - 1) {
    currentImageIndex.value++;
  } else {
    currentImageIndex.value = 0;
  }
};

const goToImage = (index) => {
  currentImageIndex.value = index;
};

const currentImage = computed(() => product.value?.images[currentImageIndex.value]);

const formatCurrency = (value) => {
  return new Intl.NumberFormat('pt-BR', {
    style: 'currency',
    currency: 'BRL',
  }).format(value);
};
</script>

<style scoped>
.image-gallery {
  position: relative; /* For positioning arrows relative to the container */
  display: flex;
  align-items: center;
  justify-content: center;
}

.image-gallery img {
  max-width: 100%; /* Ensures image scales properly */
  height: auto; /* Maintain aspect ratio */
  border-radius: 8px; /* Rounded corners */
}

button {
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10; /* Ensure buttons appear above the image */
  opacity: 0.7; /* Semi-transparent buttons */
  transition: opacity 0.3s, background-color 0.3s;
}

button:hover {
  opacity: 1; /* Fully visible on hover */
}

button:focus {
  outline: none; /* Remove focus outline */
}

.prose {
  line-height: 1.75;
  color: #4a4a4a;
}

.prose h1,
.prose h2,
.prose h3,
.prose h4,
.prose h5,
.prose h6 {
  font-weight: 700;
  margin-top: 1.5rem;
  margin-bottom: 0.75rem;
  padding-bottom: 0.25rem;
}

.prose p {
  margin-bottom: 1.5rem;
}

.prose a {
  color: #3182ce;
  text-decoration: underline;
}

.prose a:hover {
  text-decoration: none;
  color: #2b6cb0;
}

.prose blockquote {
  border-left: 4px solid #d2d6dc;
  padding-left: 1rem;
  font-style: italic;
  color: #4a5568;
}

.prose img {
  max-width: 100%;
  height: auto;
  border-radius: 0.375rem;
}
</style>
