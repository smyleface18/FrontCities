<template>
    <div>
		<CardCity client:load :refresh="value" ></CardCity>
		<AddCity client:load @SendData="getValue"></AddCity>
    </div>
</template>
<script>
import CardCity from './CardCity.vue';
import AddCity from './AddCity.vue';

export default{
    name: 'MainCards',
    components: {
        CardCity,
        AddCity
    },
    data(){
        return {
            value:null
        };
    },
    methods:{
        getValue(data){
            this.value =data ; 
        }
    }
 }


</script>