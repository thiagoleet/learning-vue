<template>
  <div>
    <h1>{{ titulo }}</h1>
    <img v-bind:src="foto.url" v-bind:alt="foto.titulo">
  </div>
</template>

<script>
export default {
  data() {
    return {
      titulo: "Alurapic",
      foto: {
        url:
          "https://encrypted-tbn2.gstatic.com/images?q=tbn:ANd9GcTwV4kVzT5McBdGSgqlVeRzubrNH_mOrrkKseDOGFURq20HmsrelEfMU7It",
        titulo: "Cachorro"
      }
    };
  }
};
</script>

<style>

</style>
