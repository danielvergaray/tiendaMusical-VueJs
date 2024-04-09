<script setup>
import { reactive, ref, onMounted, watch } from "vue";
import { db } from "./data/guitarras";
import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

/* USANDO REF */

const guitarras = ref([]);
const carrito = ref([]); //State
const guitarra = ref({});

watch(carrito, ()=>{
  guardarLocalStorage()
}, {
  deep: true
})

onMounted(() => {
  guitarras.value = db;
  guitarra.value =
    db[3]; /* Hace referencia ala guitarra modelo VAI en la posicion 3 del array de guitarras, es parte del hero */

    const carritoStorage = localStorage.getItem('carrito')
    if (carritoStorage){
      carrito.value = JSON.parse(carritoStorage) /* Se vuelve a convertir a array */
    }

  });

const guardarLocalStorage = () => {
  localStorage.setItem('carrito', JSON.stringify(carrito.value)) /* Se convierte el arreglo carrito en string */
}

const agregarCarrito = (guitarra) => {
  /* Se recibe guitarra del hijo */
  const existeCarrito = carrito.value.findIndex(
    (producto) => producto.id === guitarra.id
  );

  if (existeCarrito >= 0) {
    carrito.value[existeCarrito].cantidad++;
  } else {
    guitarra.cantidad = 1;
    carrito.value.push(guitarra);
  }

  
};

const decrementarCantidad = (id) => {
  const index = carrito.value.findIndex((producto) => producto.id === id);
  if (carrito.value[index].cantidad <= 1) return;
  carrito.value[index].cantidad--;
};

const incrementarCantidad = (id) => {
  const index = carrito.value.findIndex((producto) => producto.id === id);
  if (carrito.value[index].cantidad >= 5) return;
  carrito.value[index].cantidad++;
};

const eliminarCarrito = (id) => {
  carrito.value = carrito.value.filter((producto) => producto.id !== id);
};

const vaciarCarrito = () =>{
  carrito.value=[]
}
</script>

<template>
  <Header
    :carrito="carrito"
    :guitarra="guitarra"
    @decrementar-cantidad="decrementarCantidad"
    @incrementar-cantidad="incrementarCantidad"
    @agregar-carrito="agregarCarrito"
    @eliminar-carrito="eliminarCarrito"
    @vaciar-carrito="vaciarCarrito"
  />

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitarra
        v-for="guitarra in guitarras"
        v-bind:guitarra="guitarra"
        @agregar-carrito="agregarCarrito"
      />
      <!-- v-bind:nombreProp = "elemento a evniar por prop" -->
    </div>
  </main>

  <Footer />
</template>
