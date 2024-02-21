<script setup>
import { ref } from 'vue'
import CreateDispatch from "./components/CreateDispatch.vue";
import Dashboard from "./components/Dashboard.vue";
import Clients from "./components/Clients.vue";
import Statistics from "./components/Statistics.vue";
import CreateClient from "./components/CreateClient.vue";
const type = ref(1);
const update = () => {
  window.location.href="/"
}
const clients = () => {
  type.value =1;
}
const create = () => {
  type.value =2;
}
const statistics = () => {
  type.value =3;
}
const createClient = () => {
  type.value =4;
}
</script>

<template>
  <div class="main">
    <Dashboard @create="create" @clients="clients" @statistics="statistics" @create-client="createClient"/>
    <CreateDispatch @close="clients" v-if="type==2"/>
    <Clients v-if="type==1"/>
    <Statistics v-if="type==3"/>
    <CreateClient v-if="type==4" @close="clients"/>
  </div>
</template>

<style>
.main {
  width: 100vw;
  display: flex;
  justify-content: left;
  height: 100%;
}
#app {
  max-width: 1920px!important;
  height: 200vh;
  margin: 0!important;
  padding: 0!important;
  background-color: white;
}
</style>
