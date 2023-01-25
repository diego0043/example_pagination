<script setup>
defineProps(["title", "id", "body", "colorText"]);
</script>
<template>
  <div class="card mb-2 shadow">
    <div class="card-body">
      <h5 class="card-title text-black"> {{ id }} - {{ title }}</h5>
      <p :class="`text-${colorText}`">
        {{ body }}
      </p>
      <button @click="$emit('editFav', title)" class="btn btn-outline-success">
        Favorito
      </button>
    </div>
  </div>
  <br />
</template>
<style scoped>
.card {
  width: 40rem;
  display: block;
  margin-right: auto;
  margin-left: auto;
}
</style>
