<template>
  <div>
    hi
  </div>
</template>

<script>
export default {
  name: "index",
  data() {
    return {
      results: null,

    }
  },
  created() {
    parseInt(this.$route.params.character_id)
    if (
      this.$route.params.character_id !== undefined &&
      !isNaN(this.$route.params.character_id)
    ) {
      let character_id = parseInt(this.$route.params.character_id);

      this.getData(character_id);
    }

  },
  methods: {
    getData(character_id) {
      this.loading = true;
      this.$axios.$get("https://gateway.marvel.com/v1/public/characters/" + character_id, {
        params: {
          ts: 1,
          apikey: "78b447af6136f7b546b974a1461ecf61",
          hash: "5ace13da8a874e4080eff29c219043c3"
        }
      })
      .then((response) =>{
        console.log("response")
        console.log(response)
        this.results= response.data.results;
      })
    },


  }
}
</script>

<style scoped>

</style>
