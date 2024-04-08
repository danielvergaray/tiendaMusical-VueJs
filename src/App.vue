<script setup>
import { reactive, ref, onMounted } from "vue";
import { db } from "./data/guitarras";
import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

/* USANDO REF */

const guitarras = ref([]);
const carrito = ref([]); //State

onMounted(() => {
  guitarras.value = db;
});

const agregarCarrito = (guitarra) => {
  /* Se recibe guitarra del hijo */
  const existeCarrito= carrito.value.findIndex(producto => producto.id=== guitarra.id);

  if(existeCarrito >= 0){
    carrito.value[existeCarrito].cantidad++
  }
  else{
    guitarra.cantidad = 1;
   carrito.value.push(guitarra);
  }
  
};

const decrementarCantidad =()=>{
  console.log("menos")

}

const incrementarCantidad =()=>{
  console.log("mas")
}

</script>

<template>
  <Header
  :carrito="carrito"
  @decrementar-cantidad="decrementarCantidad"
  @incrementar-cantidad="incrementarCantidad"
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
  
<Footer/>
  
</template>
