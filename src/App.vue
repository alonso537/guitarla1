<script setup>
  import {ref,  onMounted, watch} from 'vue'
  import {db} from './data/guitarras'
  import Guitarra from './components/Guitarra.vue'
  import Header from './components/Header.vue'
  import Footer from './components/Footer.vue'
  

  const guitarras = ref([])
  const carrito = ref([])
  const guitarra = ref({})
  // console.log(state.guitarras);
  // console.log(guitarras.value);

  watch(carrito, () => {
    guardarLocalstorage()
  }, {
    deep: true
  })

  onMounted(() => {
    // console.log('mounted');
    guitarras.value = db
    guitarra.value = db[3]

    // buscar en localstorage
    const carritoLocal = localStorage.getItem('carrito')
    //si existe carrito en localstorage lo guardamos en el carrito
    if(carritoLocal){
      carrito.value = JSON.parse(carritoLocal)
    }
  })

  const guardarLocalstorage = () => {
    localStorage.setItem('carrito', JSON.stringify(carrito.value))
  }

  const AgregarCarrito = (guitarra) => {
    // alert('hola')
    const exiteCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)

    // console.log(exiteCarrito);
    if(exiteCarrito >= 0){
      carrito.value[exiteCarrito].cantidad++
      
    } else {

        guitarra.cantidad = 1
        carrito.value.push(guitarra)
    }

}

const decrementar = (id) => {
    // console.log('decrementar');
    const index = carrito.value.findIndex(producto => producto.id === id)
    if(carrito.value[index].cantidad <=1 ) return
    carrito.value[index].cantidad--

    
}

const incrementar = (id) => {
    // console.log('incrementar');
    const index = carrito.value.findIndex(producto => producto.id === id)
    carrito.value[index].cantidad++

    
}

const eliminarProducto = (id) => {
    // console.log('eliminarProducto');
   carrito.value = carrito.value.filter(producto => producto.id !== id)
}

const vaciarCarrito = () => {
    // console.log('vaciarCarrito');
    carrito.value = []

    
}

</script>

<template>
  <Header 
  :carrito="carrito"
  :guitarra="guitarra"
    @decrementar="decrementar"
    @incrementar="incrementar"
    @agregar-carrito="AgregarCarrito"
    @eliminar-producto="eliminarProducto"
    @vaciar-carrito="vaciarCarrito"
   />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <Guitarra v-for="guitarra in guitarras"  :key="guitarra.id"
            :guitarra="guitarra"
            @agregar-carrito="AgregarCarrito"
              />
            
        </div>
    </main>


   <Footer />
</template>


