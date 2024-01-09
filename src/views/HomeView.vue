<script setup>
  import { ref, onMounted, onBeforeUnmount, computed, watchEffect } from 'vue';
  import axios from 'axios';
  import { RouterLink, RouterView } from 'vue-router'

  const products = ref([]);
  onMounted(async () => {
    try {
      const response = await axios.get('https://fakestoreapi.com/products');
      products.value = response.data;
    } catch (error) {
      console.error('Error al cargar los productos:', error);
    }
  });

  const productoSeleccionado = ref(null);

  // Función para mostrar los detalles del producto en el modal
  const mostrarDetalles = (product) => {
    productoSeleccionado.value = product;
  };

  // Función para cerrar el modal
  const cerrarModal = () => {
    productoSeleccionado.value = null;
  };
</script>

<template>
  <div class="grid grid-cols-12 gap-6">
    <div class="col-span-12 lg:col-span-2 2xl:col-span-2 bg-white m-2 rounded-lg">
      <div class="rounded-lg border border-gray-300 py-4">
          <h2 class="text-center font-black text-2xl mt-2 custom-element">
            Filtrar
          </h2>
      </div>
    </div>

    <div class="col-span-12 lg:col-span-10 2xl:col-span-10 bg-white m-2 rounded-lg">

      <div class="relative z-20 flex items-center overflow-hidden bg-white dark:bg-gray-800">
          <div class="container relative flex px-6 py-16 mx-auto">
              <div class="relative z-20 flex flex-col sm:w-2/3 lg:w-2/5">
                  <span class="w-20 h-2 mb-12 bg-gray-800 dark:bg-white">
                  </span>
                  <h1 class="flex flex-col text-6xl font-black leading-none text-gray-800 uppercase font-bebas-neue sm:text-8xl dark:text-white">
                      Be on
                      <span class="text-5xl sm:text-7xl">
                          Time
                      </span>
                  </h1>
                  <p class="text-sm text-gray-700 sm:text-base dark:text-white">
                      Dimension of reality that makes change possible and understandable. An indefinite and homogeneous environment in which natural events and human existence take place.
                  </p>
                  <div class="flex mt-8">
                      <a href="#" class="px-4 py-2 mr-4 text-white uppercase bg-pink-500 border-2 border-transparent rounded-lg text-md hover:bg-pink-400">
                          Get started
                      </a>
                      <a href="#" class="px-4 py-2 text-pink-500 uppercase bg-transparent border-2 border-pink-500 rounded-lg dark:text-white hover:bg-pink-500 hover:text-white text-md">
                          Read more
                      </a>
                  </div>
              </div>
              <div class="relative hidden sm:block sm:w-1/3 lg:w-3/5">
                  <img src="https://www.tailwind-kit.com/images/object/10.png" class="max-w-xs m-auto md:max-w-sm"/>
              </div>
          </div>
      </div>

      <h2 class="text-center font-black text-2xl mt-2 custom-element">
          Productos
      </h2>

      <div class="w-full sm:w-auto flex justify-end mt-4 mr-4">
          <div class="relative text-gray-600">
            <input type="search" name="serch" placeholder="Search" class="h-10 px-5 pr-10 rounded-full text-sm focus:outline-none bg-gray-200">
            <span type="submit" class="absolute right-0 top-0 mt-3 mr-4">
              <svg class="h-4 w-4 fill-current" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" id="Capa_1" x="0px" y="0px" viewBox="0 0 56.966 56.966" style="enable-background:new 0 0 56.966 56.966;" xml:space="preserve" width="512px" height="512px">
                <path d="M55.146,51.887L41.588,37.786c3.486-4.144,5.396-9.358,5.396-14.786c0-12.682-10.318-23-23-23s-23,10.318-23,23  s10.318,23,23,23c4.761,0,9.298-1.436,13.177-4.162l13.661,14.208c0.571,0.593,1.339,0.92,2.162,0.92  c0.779,0,1.518-0.297,2.079-0.837C56.255,54.982,56.293,53.08,55.146,51.887z M23.984,6c9.374,0,17,7.626,17,17s-7.626,17-17,17  s-17-7.626-17-17S14.61,6,23.984,6z"/>
              </svg>
            </span>
          </div>
      </div>

      <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 ml-4 mr-4">
        <div v-for="product in products" :key="product.id">

          <div class="intro-y mx-auto max-w-2xl px-4 py-4 sm:px-6 sm:py-12 lg:max-w-7xl lg:px-8">
            <div class="drop-shadow-md">
              <div href="#" class="group animateanimated animatebounceInUp">
                <div class="aspect-h-1 aspect-w-1 w-full overflow-hidden rounded-lg bg-gray-200 xl:aspect-h-8 xl:aspect-w-7">
                  <img :src="product.image" class="h-64 w-64 object-cover object-center group-hover:opacity-75"/>
                </div>
                <h3 class="drop-shadow-md mt-4 font-bold text-md text-gray-700">{{ product.title }}</h3>
                <h4 class="mb-4 text-xs text-slate-400">{{ product.category }}</h4>
                <p class="mt-1 text-lg font-medium text-gray-900">${{ product.price }}</p>
              </div>
              <button class="btn bg-blue-700 w-full rounded-full pt-2 pb-2 text-white" @click="mostrarDetalles(product)">Ver Detalles</button>
            </div>
          </div>

        </div>
      </div>

      <div class="border-b border-gray-300 mt-4 mb-4"></div>
      
    </div>

  </div>

  <div v-if="productoSeleccionado" class="modal">

    <div class=" border border-r-4 flex min-h-full items-stretch justify-center text-center md:items-center shadow-2xl">
      <div class="flex w-full transform text-left text-base transition md:max-w-2xl md:px-4 lg:max-w-4xl">
        <div class="relative flex w-full items-center overflow-hidden bg-white px-4 pb-8 pt-14 sm:px-6 sm:pt-8 md:p-6 lg:p-8">
          <div class="grid w-full grid-cols-1 items-start gap-x-6 gap-y-8 sm:grid-cols-12 lg:gap-x-8">
            
            <!--Imagen del producto-->
            <div class="aspect-h-3 aspect-w-2 overflow-hidden rounded-lg bg-gray-100 sm:col-span-4 lg:col-span-5">
              <img :src="productoSeleccionado.image" alt="">
            </div>

            <!--Información del producto-->
            <div class="sm:col-span-8 lg:col-span-7">
              <h2 class="text-2xl font-bold text-gray-900 sm:pr-12">{{ productoSeleccionado.title }}</h2>
              <section aria-labelledby="information-heading" class="mt-2">
                <div class="border-b border-gray-300 mt-4 mb-4"></div>
                <p class="text-2xl text-gray-900">${{ productoSeleccionado.price }}</p>
                <div class="border-b border-gray-300 mt-4 mb-4"></div>
                <p class="text-base text-gray-900 text-justify">{{productoSeleccionado.description }}</p>
                <div class="border-b border-gray-300 mt-4 mb-4"></div>
              </section>
              <button class="btn btn-primary mt-6 flex w-full items-center justify-center rounded-md border bg-blue-700 px-8 py-3 text-base font-medium text-white">
                Agregar
              </button>

              <button class="btn btn-primary mt-6 flex w-full items-center justify-center rounded-md border bg-red-700 px-8 py-3 text-base font-medium text-white" @click="cerrarModal">
                Cerrar
              </button>
            </div>

          </div>
        </div>
      </div>
    </div>

  </div>

</template>


<style>
/* Estilos para el modal (puedes personalizarlos) */
.modal {
  display: block;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 999;
}
</style>