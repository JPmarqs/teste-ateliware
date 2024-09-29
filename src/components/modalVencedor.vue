<template>
  <div class="card flex justify-center">
    <Dialog
      header="Vencedor!"
      v-model:visible="visible"
      :style="{ width: '25rem' }"
    >
      <div>
        <h2>{{ pokemon }}</h2>
        <Image
          v-if="imagem"
          :src="imagem"
          alt="Image"
          width="250"
          preview
        />
      </div>
    </Dialog>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";
import Image from "primevue/image";
import Dialog from "primevue/dialog";

const props = defineProps({
  pokemon: String,
  imagem: String,
  abrirModal: Boolean,
});

const visible = ref(false);

watch(props, () => {
  visible.value = props.abrirModal;
});
</script>
