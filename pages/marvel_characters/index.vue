<template>
 <div>
   <a-card title="Default size card" style="width: 300px">
     <a slot="extra" href="#">more</a>
     <p>card content</p>
     <p>card content</p>
     <p>card content</p>
   </a-card>

   <a-card hoverable style="width: 240px">
     <img
       slot="cover"
       alt="example"
       src="https://os.alipayobjects.com/rmsportal/QBnOOoLaAfKPirc.png"
     />
     <a-card-meta title="Europe Street beat">
       <template slot="description">
         www.instagram.com
       </template>
     </a-card-meta>
   </a-card>
 </div>
</template>

<script>
import 'ant-design-vue/dist/antd.css';

export default {
  name: "index",
  data() {
    return{
      results: null
    }
  },
  created() {
    this.getData();
  },
  methods: {
    getData(){
      this.$axios.$get("https://gateway.marvel.com/v1/public/characters", {
        params: {
          ts: 1,
          apikey: "78b447af6136f7b546b974a1461ecf61",
          hash: "5ace13da8a874e4080eff29c219043c3"
        }
      })
      .then((response) => {
        console.log("response")
        console.log(response)
        this.results = response.data.results;
      })
    }
  }
}
</script>

<style scoped>

</style>
