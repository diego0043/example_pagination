<script setup>
import BlogPost from "./components/BlogPost.vue";
import PaginationComponent from "./components/PaginationComponent.vue";
import { ref, onMounted } from "vue";
const ArrayData = ref([]);
const fav = ref("");
const cargando = ref(true);
const limit = ref(0);
const limitInf = ref(true);
const limitSup = ref(false);
const editFav = (title) => {
  fav.value = title;
};
const editLimit = () => {
  limit.value = limit.value + 10;
  limit.value >= ArrayData.value.length - 10
    ? (limitSup.value = true)
    : (limitSup.value = false);
  limit.value === 0 ? (limitInf.value = true) : (limitInf.value = false);
};
const decrementLimit = () => {
  limit.value = limit.value - 10;
  limit.value === 0 ? (limitInf.value = true) : (limitInf.value = false);
  limit.value === 90 ? (limitSup.value = true) : (limitSup.value = false);
};

const getData = async () => {
  try {
    const data = await fetch("https://jsonplaceholder.typicode.com/posts");
    const dataJson = await data.json();
    ArrayData.value = [...dataJson];
    cargando.value = false;
  } catch {
    console.log("error");
  } finally {
    cargando.value = false;
  }
};

getData();
//fetch("https://jsonplaceholder.typicode.com/posts")
//.then((res) => res.json())
//.then((data) => (ArrayData.value = [...data])).finally( () => cargando.value = false);
</script>
<template>
  <div
    v-show="cargando"
    class="spinner-border text-primary cargando"
    role="status"
  >
    <span class="visually-hidden">Loading...</span>
  </div>
  <h1 v-show="!cargando" class="title-pri">Tab favorito: {{ fav }}</h1>
  <PaginationComponent
    class="pagination-prop"
    @editLimit="editLimit"
    @decrementLimit="decrementLimit"
    :limitInf="limitInf"
    :limitSup="limitSup"
  />
  <BlogPost
    v-for="item in ArrayData.slice(limit, limit + 10)"
    :key="item.id"
    :title="item.title"
    :id="item.id"
    :body="item.body"
    :colorText="item.colorText"
    @editFav="editFav"
  />
</template>
<style scoped>
.title-pri {
  text-align: center;
}

.loading {
  display: inline-block;
  margin-left: 48vw;
  margin-top: 40vh;
}

.pagination-prop {
  display: inline-block;
  margin-left: 43vw;
  margin-bottom: 2vh;
  margin-top: 2vh;
}

.cargando {
  display: block;
  margin-left: 48vw;
  margin-top: 40vh;
}
</style>
