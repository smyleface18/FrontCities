<template>
    <div class="w-[100%] h-[100%] flex flex-col justify-center items-center">
      <input type="file" @change="onFileChange" class="w-[100%] h-[50%]"/>
      <button @click="submitFile" class="bg-[#13151a] rounded hover:text-teal-500 w-[2em] ">
        <svg xmlns="http://www.w3.org/2000/svg" width="2em" height="2em" viewBox="0 0 50 50"><g fill="none" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"><path stroke="#14b8a6" d="M25 25v18.75m6.479-14.625L24.854 22.5l-6.334 6.333"/><path stroke="#14b8a6" d="M12.5 31.48a13.73 13.73 0 1 1 20.833-15.084a9.6 9.6 0 0 1 1.73-.167A8.73 8.73 0 0 1 37.5 33.333"/></g></svg>
      </button>
    </div>
  </template>
  
  <script>
  import axios from 'axios';

  export default {
    props: {
    id: {
      type: Number,
      required: true
        }
    },
    data() {
      return {
        selectedFile: null,
        refresh: false
      };
    },
    methods: {

      onFileChange(event) {
        this.selectedFile = event.target.files[0];
      },
      refreshvalue(){
        this.refresh = !this.refresh
        this.$emit('SendData', this.refresh);
      },

      submitFile() {
        if (!this.selectedFile) {
          alert("Please select a file first");
          return;
        }
        const formData = new FormData();
        formData.append('file', this.selectedFile);
        formData.append('id', this.id);  
  
        // Realizamos la petición POST con Axios
        axios.post('http://localhost:8080/api/workers/upload', formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        })
        .then(response => {
          this.refreshvalue();
        })
        .catch(error => {
          console.error(error);
          alert("Error uploading file");
        });
      }
    }
  };

  </script>