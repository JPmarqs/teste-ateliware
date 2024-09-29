<template>
  <div class="topo">
    <img
      alt="Vue logo"
      class="logo"
      src="../assets/logopokemon.png"
      width="150"
      height="150"
    />
    <h3 class="title">Batalha de Pokemons</h3>
  </div>
  <div class="conteudo">
    <Selecao-de-pokemons :pokemons="pokemons"></Selecao-de-pokemons>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import api from "../service/index";
import SelecaoDePokemons from "../components/SelecaoDePokemons.vue";

const pokemons = ref([]);

const fetchPokemons = async () =>
  await api
    .get("https://pokeapi.co/api/v2/pokemon/?limit=20&offset=20")
    .then((res) => (pokemons.value = res.data.results));

onMounted(() => {
  fetchPokemons();
});
</script>

<style scoped>
.conteudo {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.topo {
  display: flex;
  justify-content: start;
  align-items: center;
  border-bottom: 1px solid #ccc;
}

.logo {
  margin: 0 auto;
  width: 125px;
  height: 125px;
  margin-left: auto;
  margin-right: auto;
}
.title {
  margin-right: auto;
}
</style>
