<template>
  <div class="container mx-auto p-6 space-y-12">
    <!-- Header Section -->
    <div class="text-center space-y-4">
      <h1 class="text-4xl font-bold text-gray-800">Venda de garagem do Nicholas</h1>
      <p class="text-lg text-gray-600 max-w-2xl mx-auto">
        Estou vendendo itens que não uso mais e estão em excelente condição.
        Entrando em cada item você pode ver mais fotos e detalhes.
      </p>
    </div>

    <!-- Information List -->
    <PaymentInfo></PaymentInfo>

    <!-- Product Grid -->
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-8">
      <div
        v-for="product in products"
        :key="product.id"
        class="card border border-gray-200 shadow-lg rounded-lg overflow-hidden hover:shadow-xl transition-shadow duration-300"
      >
        <!-- Wrap the image in a clickable link -->
        <router-link
          :to="'/product/' + product.slug"
          class="relative group block"
        >
          <!-- Thumbnail Wrapper -->
          <div
            class="relative overflow-hidden bg-gray-50"
            style="aspect-ratio: 4 / 3;"
          >
            <!-- Product Image -->
            <img
              :src="product.images[0]"
              alt="Imagem do Produto"
              class="w-full h-full object-cover"
            />
          </div>

          <!-- Overlay on hover -->
          <div
            class="absolute inset-0 bg-black bg-opacity-30 opacity-0 group-hover:opacity-100 transition-opacity duration-300 flex items-center justify-center"
          >
            <span class="text-white font-semibold text-lg">Ver detalhes</span>
          </div>
        </router-link>

        <!-- Product Info -->
        <div class="p-4 space-y-4 bg-white">
          <h2 class="text-xl font-bold text-gray-800">{{ product.title }}</h2>
          <p class="text-lg text-gray-600">{{ formatCurrency(product.price) }}</p>
          <router-link
            :to="'/product/' + product.slug"
            class="inline-block px-4 py-2 bg-blue-600 text-white font-semibold rounded-lg hover:bg-blue-700 transition"
          >
            Ver detalhes
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import PaymentInfo from '~/components/PaymentInfo.vue';
import customHead from '~/components/CustomHead.vue';

customHead();

const products = ref([]);

onMounted(async () => {
  const listProducts = await queryContent('products').find();
  products.value = listProducts;
});

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
</style>
