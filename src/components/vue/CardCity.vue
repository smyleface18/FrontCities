<template>
    <div class="w-[100%] h-[100%]  flex flex-col justify-center items-center">
        <div class="flex justify-center items-center space-x-8 mb-10">
            <h1 class="text-5xl  ">CIUDADES DE COLOMBIA</h1>
            <img class="size-16 rounded-3xl" src="https://i.pinimg.com/564x/7b/6c/66/7b6c66bf64328c47c3de03c0b8c7f84f.jpg" alt="">
        </div>
      <div id="main-container"  v-for="city in  DATAJSON" class="h-56 w-[100%] text-white shadow-md bg-[#282b32] p-10 flex flex-col items-center">
        <div id="container" class=" flex h-[100%] " >
            <ul class="w-[100%] h-[100%] flex items-center justify-center space-x-10 mr-5">
                <li>
                    <h3 class="text-red-500 font-semibold ">Ciudad</h3>
                    <h4>{{ city.name }}</h4>
                </li>
                <li>
                    <h3 class="text-red-500 font-semibold ">Departamento</h3>
                    <h4>{{ city.district }}</h4>
                </li>
                <li>
                    <h3 class="text-red-500 font-semibold ">Población</h3>
                    <h4>{{ fomart(city.population)}}</h4>
                </li>
            </ul>
            <div class="h-[100%] w-[50%] overflow-hidden ">
                <upImg client:load @sendData="refreshImg" :id="city.id " v-if="city.img == null" class="w-[100%] h-[100%]"></upImg>
                <img v-if="city.img != null" class="object-cover" :src="city.img" :alt="'foto de la ciudad de '+city.name+' en colombia ' " >
                    
            </div>
        </div >
            <a :href="'https://www.google.com/search?q=' + city.name" class=" font-semibold px-10 bg-[#13151a] text-teal-500 hover:text-[#13151a] hover:bg-teal-500">Buscar Ciudad</a>
      </div>
    </div>

</template>
<style>

    @media (max-width: 768px) {
        #main-container {
            height: 350px;
        }
        #container {
        flex-direction: column;
        margin-bottom: 10px;
        }
        #container > ul {
            margin-bottom: 20px;
        }
    }
</style>


<script setup>
import UpImg from './UpImg.vue';
import { ref, watch, onMounted } from 'vue';


let props = defineProps({
  refresh: Boolean,
});
let DATAJSON = ref([]);
let reImg = ref();



// Función para obtener la data de la API
async function fetchCities() {
  try {
    const response = await fetch('http://localhost:8080/api/workers/Citys');
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const data = await response.json();
    DATAJSON.value = data; // Actualiza el valor de DATAJSON
  } catch (error) {
    console.error('Error fetching cities:', error);
  }
}

// Se ejecuta al montar el componente
onMounted(() => {
  fetchCities();
});

    function fomart( population){
     return   population.toLocaleString('en');
    }

    function refreshImg(data){
        reImg.value = data;
    }

    watch(() => props.refresh, (newValue) => {
        fetchCities();
        
    });

    watch(() => reImg.value, (newValue) => {
        fetchCities();
        
    });
    
</script>
