<template>
  <div class="container">
    <div class="layout-form custom-width lg trade_marks">
      <h6 class="mb-5">
        <NuxtLink class="main-title normal lg" href="/">{{
          $t("Titles.home")
        }}</NuxtLink>
         <i class="fas fa-chevron-left change-angle"></i> 
        <span class="main-title normal lg">{{ $t(`${route.name}`) }}</span>
      </h6>
      <div class="row" v-if="loading">
        <div class="col-12 col-md-3 mb-4" v-for="i in 4" :key="i">
          <Skeleton height="12rem"></Skeleton>
        </div>
      </div>
      <div class="row">
        <NuxtLink
          v-for="brand in brands"
          :to="'/trade_mark' + brand.id"
          class="col-12 col-md-3 mb-4 div-img"
        >
          <img :src="brand.logo" alt="" />
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<script setup>
definePageMeta({
  name: "Titles.trade_marks",
});
const axios = useApi();
const route = useRoute();
const brands = ref([]),
  loading = ref(true);
onMounted(() => {
  axios
    .get("brands", {
      headers: {
        lang: `${localStorage.getItem("locale") == "en" ? "en" : null}`,
      },
    })
    .then((res) => {
      brands.value = res.data.data.data;
      loading.value = false;
    });
});
</script>

<style lang="scss" scoped>
.trade_marks {
  text-align: start;
}
.div-img {
  transition: all 0.3s;
  padding: 0 0.5em;
  @media (max-width: 770px) {
    padding: 0;
  }
  img {
    width: 100%;
    height: 200px;
  }
  &:hover {
    transform: scale(1.05);
  }
}
</style>
