<template>
  <div class="container mx-auto p-6 space-y-12">
    <!-- Header Section -->
    <div class="text-center space-y-4">
      <h1 class="text-4xl font-bold text-gray-800">Venda de garagem do Nicholas</h1>
      <p class="text-lg text-gray-600 max-w-2xl mx-auto">
        Estou de mudança e vendendo itens que não tenho como levar mas estão em excelente condição.
        Entrando em cada item você pode ver mais fotos e detalhes.
      </p>
    </div>

    <!-- Information List -->
    <PaymentInfo></PaymentInfo>

    <!-- Product Grid -->
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-8">
      <div v-for="product in products" :key="product.id"
        class="card border border-gray-200 shadow-lg rounded-lg overflow-hidden hover:shadow-xl transition-shadow duration-300">
        <!-- Wrap the image in a clickable link only if not sold -->
        <router-link v-if="!product.sold" :to="'/product/' + product.slug" class="relative group block">
          <!-- Thumbnail Wrapper -->
          <div class="relative overflow-hidden bg-gray-50" style="aspect-ratio: 4 / 3;">
            <!-- Product Image -->
            <img :src="product.images[0]" alt="Imagem do Produto" class="w-full h-full object-cover" />
          </div>

          <!-- Overlay on hover -->
          <div
            class="absolute inset-0 bg-black bg-opacity-30 opacity-0 group-hover:opacity-100 transition-opacity duration-300 flex items-center justify-center">
            <span class="text-white font-semibold text-lg">Ver detalhes</span>
          </div>
        </router-link>

        <!-- Sold Overlay -->
        <div v-else class="relative overflow-hidden bg-gray-50" style="aspect-ratio: 4 / 3;">
          <img :src="product.images[0]" alt="Imagem do Produto" class="w-full h-full object-cover opacity-50" />
          <div class="absolute inset-0 flex items-center justify-center bg-black bg-opacity-50">
            <span class="text-white font-bold text-xl">Vendido</span>
          </div>
        </div>

        <!-- Product Info -->
        <div class="p-4 space-y-4 bg-white">
          <h2 class="text-xl font-bold text-gray-800">{{ product.title }}</h2>
          <p v-if="!product.sold" class="text-lg text-gray-600">
            {{ formatCurrency(product.price) }}
          </p>
          <p v-else class="text-lg font-semibold text-red-600">
            Não disponível - vendido
          </p>

          <!-- Ver detalhes button only if not sold -->
          <router-link v-if="!product.sold" :to="'/product/' + product.slug"
            class="inline-block px-4 py-2 bg-blue-600 text-white font-semibold rounded-lg hover:bg-blue-700 transition">
            Ver detalhes
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup async>
import { ref, onMounted } from 'vue';
import PaymentInfo from '~/components/PaymentInfo.vue';
import customHead from '~/components/CustomHead.vue';

customHead();

const products = ref([]);
const { data: listProducts } = await useAsyncData('listProducts', () => queryContent('products').find());
products.value = listProducts.value;

// Currency formatting function
const formatCurrency = (value) => {
  return new Intl.NumberFormat('pt-BR', {
    style: 'currency',
    currency: 'BRL',
  }).format(value);
};
</script>

<style scoped>
img {
  object-fit: cover;
  object-position: center;
}

.card {
  border-radius: 8px; /* Smooth corners */
  transition: border-color 0.3s, box-shadow 0.3s;
}

.card:hover {
  border-color: #4b5563; /* Dark gray (tailwind's gray-600) */
  box-shadow: 0 6px 12px rgba(75, 85, 99, 0.2); /* More pronounced shadow */
}

.bg-gray-50 {
  background-color: #f9fafb; /* Light gray background */
}

.border {
  border-width: 1px;
}

.shadow-lg {
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.hover\:shadow-xl:hover {
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

.card img.opacity-50 {
  filter: grayscale(100%); /* Optional: make the image grayscale for sold items */
  opacity: 0.5;
}

.card .bg-black.bg-opacity-50 {
  background: rgba(0, 0, 0, 0.5); /* Semi-transparent dark overlay */
}

.card .text-xl {
  font-size: 1.25rem; /* Adjust the size of the "Vendido" text */
}
</style>
