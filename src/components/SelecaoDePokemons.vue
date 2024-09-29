<template>
  <h2>Selecione os pokemons para a batalha!</h2>
  <div class="selecao-pokemon">
    <div class="flex-jc__se-ai-center">
      <Select
        v-model="primeiroPokemon"
        :options="pokemons"
        optionLabel="name"
        placeholder="Select a Pokemon"
        class="w-full md:w-56"
      />
      <div>
        <Button
          label="Batalha!"
          severity="danger"
          text
          raised
          rounded
          @click="pokemonsStat(primeiroPokemon.name, segundoPokemon.name)"
        />
      </div>
      <Select
        v-model="segundoPokemon"
        :options="pokemons"
        optionLabel="name"
        placeholder="Select a Pokemon"
        class="w-full md:w-56"
      />
    </div>
    <div class="flex-jc__se-ai-center">
      <Image
        v-if="imgPokemon1"
        :src="imgPokemon1"
        alt="Image"
        width="250"
        preview
      />
      <Progress-spinner v-if="loading" size="large" style="z-index: 1000;"/>
      <Image
      v-if="imgPokemon2"
      :src="imgPokemon2"
      alt="Image"
      width="250"
      preview
      />
    </div>
    <Modal-vencedor
      :pokemon="vencedor"
      :imagem="imgVencedor"
      :abrirModal="abrirModal"
    />
  </div>
</template>

<script setup>
import { ref } from "vue";
import Select from "primevue/select";
import Button from "primevue/button";
import Image from "primevue/image";
import ProgressSpinner from "primevue/progressSpinner";

import ModalVencedor from "./modalVencedor.vue";
import api from "../service/index";

const primeiroPokemon = ref();
const segundoPokemon = ref();
const hpPokemon1 = ref();
const hpPokemon2 = ref();
const imgPokemon1 = ref();
const imgPokemon2 = ref();
const response = ref();
const vencedor = ref();
const abrirModal = ref(false);
const imgVencedor = ref();
const loading = ref(false);

const props = defineProps({
  pokemons: Array,
});

const fetchPokemonStats = async (pokemon) =>
  await api.get(`/${pokemon}`).then((res) => (response.value = res.data));

const pokemonsStat = async (primeiroPokemon, segundoPokemon) => {
  loading.value = true;

  var primeiro = await fetchPokemonStats(primeiroPokemon);
  hpPokemon1.value = primeiro.stats[0].base_stat;
  imgPokemon1.value = primeiro.sprites.front_default;

  var segundo = await fetchPokemonStats(segundoPokemon);
  hpPokemon2.value = segundo.stats[0].base_stat;
  imgPokemon2.value = segundo.sprites.front_default;

  vencedor.value =
    hpPokemon1.value > hpPokemon2.value ? primeiroPokemon : segundoPokemon;
  imgVencedor.value =
    hpPokemon1.value > hpPokemon2.value ? imgPokemon1.value : imgPokemon2.value;

  console.log(loading.value);

  setTimeout(() => {
    if (primeiroPokemon !== "" && segundoPokemon !== "") {
      abrirModal.value = true;
    }
    loading.value = false;
  }, 2000);

  resetForm();
};

const resetForm = () => {
  primeiroPokemon.value = "";
  segundoPokemon.value = "";
  hpPokemon1.value = 0;
  hpPokemon2.value = 0;
};
</script>

<style scoped>
.flex-jc__se-ai-center {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  width: 100%;
  padding-top: 25px;
}
.selecao-pokemon {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
}
.img-pokemon {
  width: 25%;
}
</style>
