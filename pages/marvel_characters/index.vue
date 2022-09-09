<template>
 <div>
   <div class="flex">
     <div class="flex" v-for="data in results">
       <a-card hoverable :loading="loading" style="width: 240px">

         <img
           slot="cover"
           :alt="data.name"
           :src="data.thumbnail.path + '.' + data.thumbnail.extension"
         />
         <a-card-meta>
           <template slot="title">
             <NuxtLink :to=" '/marvel_characters/' + data.id + '/view/'">
              {{data.name}}
             </NuxtLink>
           </template>

           <template slot="description">
             {{ data.desciption }}
           </template>
         </a-card-meta>

       </a-card>
     </div>

   </div>

 </div>
</template>

<script>
export default {
  name: "index",
  data() {
    return{
      results: null,
      loading: false,
    }
  },
  created() {
    this.getData();
  },
  methods: {
    getData(){
      this.loading = true;
      this.$axios.$get("https://gateway.marvel.com/v1/public/characters", {
        params: {
          ts: 1,
          apikey: "78b447af6136f7b546b974a1461ecf61",
          hash: "5ace13da8a874e4080eff29c219043c3"
        }
      })
      .then((response) => {
        this.results = response.data.results;
        this.loading = false;
      })
    }
  }
}
</script>

<style scoped>

</style>
