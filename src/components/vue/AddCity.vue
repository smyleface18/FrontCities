<template>
    <div class="py-10 flex flex-col justify-center items-center  ">
        <h4 class="w-[100%] flex justify-center items-center text-teal-500 pb-4 font-semibold text-3xl ">AGREGA UNA CIUDAD</h4>
        <div id="alert"   class="w-[80%] hidden bg-teal-100 border-t-4 border-teal-500 rounded-b text-teal-900 px-4 py-3 shadow-md" role="alert">
                <div class="flex justify-center space-x-40 items-center">
                    <div>
                        <p class="font-bold">La ciudad se agrego correctamente</p>
                        <p class="text-sm">El proceso fue exitoso.</p>
                    </div>
                    <button @click="hiddenAlert()">
                        <svg xmlns="http://www.w3.org/2000/svg" width="2em" height="2em" viewBox="0 0 24 24"><path fill="currentColor" d="M18.3 5.71a.996.996 0 0 0-1.41 0L12 10.59L7.11 5.7A.996.996 0 1 0 5.7 7.11L10.59 12L5.7 16.89a.996.996 0 1 0 1.41 1.41L12 13.41l4.89 4.89a.996.996 0 1 0 1.41-1.41L13.41 12l4.89-4.89c.38-.38.38-1.02 0-1.4"/></svg>
                    </button>    
                </div>
        </div>
        <div id="AlertError" class="w-[80%] hidden bg-red-200 border-t-4 border-red-600 rounded-b text-red-900 px-4 py-3 shadow-md" role="alert">
                <div class="flex justify-center space-x-40 items-center">
                    <div>
                        <p class="font-bold">La ciudad no se agrego correctamente</p>
                        <p class="text-sm">El proceso fallo</p>
                    </div>
                    <button @click="hiddenAlertError()">
                        <svg xmlns="http://www.w3.org/2000/svg" width="2em" height="2em" viewBox="0 0 24 24"><path fill="currentColor" d="M18.3 5.71a.996.996 0 0 0-1.41 0L12 10.59L7.11 5.7A.996.996 0 1 0 5.7 7.11L10.59 12L5.7 16.89a.996.996 0 1 0 1.41 1.41L12 13.41l4.89 4.89a.996.996 0 1 0 1.41-1.41L13.41 12l4.89-4.89c.38-.38.38-1.02 0-1.4"/></svg>
                    </button>    
                </div>
        </div>
        <form @submit.prevent="submitForm"  class="flex flex-col w-[80%] space-y-2">
            <input v-model="name" placeholder="Nombre" required  class="bg-black px-2" />
            <input v-model="district" placeholder="Distrito" required  class="bg-black px-2"/>
            <input v-model="population" type="number" placeholder="Población" required class="bg-black px-2" />
            <br>
            <button type="submit" class="bg-teal-700  rounded hover:bg-teal-500">Enviar</button>

        </form>
    </div>
</template>

<script>
    
export default {
    data() {
        return {
            name: '',
            district: '',
            population: '',
            value: false,
        };
    },
    methods: {
        handleFileUpload(event) {
            this.img = event.target.img[0];
        },
        submitForm() {
            fetch("http://localhost:8080/api/workers/addCity", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify({
                    name: this.name,
                    district: this.district,
                    population: this.population,
                })
            })
            .then(response => response.json())
            .then(data => {
                this.name = "";
                this.countryCode = "";
                this.district = "";
                this.population = "";
                let element = document.querySelector("#alert");
                element.classList.remove("hidden");
                this.sendData();
            })
            .catch(error => {

                this.name = "";
                this.district = "";
                this.population = "";
                let element = document.querySelector("#AlertError");
                element.classList.remove("hidden");
                console.error('Error:', error);
            });
        },

        hiddenAlert(){
        let element = document.querySelector("#alert");
        element.classList.add("hidden");
        },
        hiddenAlertError(){
        let element = document.querySelector("#AlertError");
        element.classList.add("hidden");
        },
        sendData() {
             this.value = !this.value
            this.$emit('SendData', this.value); // Emite un evento con los datos
    }

    }
};


</script>