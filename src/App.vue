<template>
  <div class="container">
    <h1 class="title-s">Search Digimon by name:</h1>
    <input v-model="name" class="name-input" type="text" placeholder="Enter name..." />
    <button @click="newName = name" class="btn-s">Search</button>
    <p>{{newName}}</p>
  </div>
  <ul class="digi-list">
    <li v-for="elem in data" :key="elem.name">
      <h4>{{elem.name}}</h4>
      <img :src="elem.img" />
    </li>
  </ul>
</template>

<script>
import {reactive, ref, watch, toRefs} from 'vue';

export default {
  name: 'App',
  setup() {
    const name = ref(null)
    const newName = ref(null)
    const state = reactive({data: []})

    watch(() => {
      if(newName.value)
      fetch(`https://digimon-api.vercel.app/api/digimon/name/${newName.value}`).then(response => response.json())
      .then(data => {
        console.log(data);
        state.data = data;
      });
    })
    return{
      name,
      newName,
      ...toRefs(state)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  justify-content: center;
  width: 50%;
  margin: auto
}

.btn-s {
  width: 20%;
  height: 32px;
  margin: auto;
  background-color: blueviolet;
  margin-top:20px;
  color: wheat;
  font-weight: bold;
  font-size: 17px;
}

.name-input {
  width: 70%;
  height: 30px;
  margin: auto;
  background-color: antiquewhite;
}
.digi-list {
   list-style: none;
}


</style>
