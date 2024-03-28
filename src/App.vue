<script  setup>
import Header from './components/Header.vue';
import Guitarra from './components/Guitarra.vue';


import {db} from './database/db'
import {reactive, ref,onMounted,watch } from 'vue';


const guitarras = ref([])
const guitarra = ref({})

const carrito = ref([])


watch(carrito,()=>{
saveLocalStorage() 

},{
    deep:true,

})
onMounted(()=>{
    guitarras.value = db
    guitarra.value = db[5]
    const carritoStorage = localStorage.getItem('carrito')
    if (carritoStorage) {
        carrito.value = JSON.parse(carritoStorage)
        
    }
})





const saveLocalStorage  =()=>{
    localStorage.setItem('carrito',JSON.stringify(carrito.value))


}

const  agregarCarrito =(guittara)=>{

    const exist = carrito.value.findIndex((item) => item.id  === guittara.id)
    
    if (exist >=0  ) {
        carrito.value[exist].cantidad++
        
    }else{
        guittara.cantidad = 1
         carrito.value.push(guittara)
    }

  
}

const decrementarCantidad = (id)=>{
    const index = carrito.value.findIndex((item) => item.id  === id)

  if (  carrito.value[index].cantidad <= 1) return 
  carrito.value[index].cantidad--;
  
  
}
const incrementarCantidad = (id)=>{
    const index = carrito.value.findIndex((item) => item.id  === id)
    if (  carrito.value[index].cantidad >= 5) return 
    carrito.value[index].cantidad++;
    
}

const removeProducts = (id) =>{
    carrito.value = carrito.value.filter(item => item.id != id)

 
}

const vaciar = () =>{
    carrito.value =[]
   
}
</script>

<template lang="html">
  <Header  

  :carrito = "carrito"
  :guitarra = "guitarra"
  @decrementar-cantidad ="decrementarCantidad"
  @incrementar-cantidad ="incrementarCantidad"
  @agregar-carrito = agregarCarrito
  @remove-products = removeProducts
  @vaciar = vaciar


  
  
  />


    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
         
        
        <Guitarra
        class=""
            v-for="guitarra in guitarras"
            :guitarra ="guitarra"
            @agregar-carrito = agregarCarrito

            
        
        />
           
            

    
        </div>
    </main>


    <footer class="bg-dark mt-5 py-5">
        <div class="container-xl">
            <p class="text-white text-center fs-4 mt-4 m-md-0">GuitarLA - Todos los derechos Reservados</p>
        </div>
    </footer
    </>
</template>

<style lang="less" scoped>
    
</style>
