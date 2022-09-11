<template>
  <div>
    <a-page-header
      title="Back"
      @back="
        () => {
          this.$router.back();
        }
      "
    >
    </a-page-header>

      <div v-for="data in results" class="flex justify-center ">

      <div class="flex flex-col p-4">
        <div class="flex justify-center">
          <img
            slot="cover"
            :alt="data.name"
            :src="data.thumbnail.path + '.' + data.thumbnail.extension"
            class="h-60 w-60"
          />
        </div>

        <div class="flex flex-col text-center text-3xl p-4">
          {{ data.name }}
        </div>

        <div>
          <a-collapse accordion v-for="data in results" style="width: 400px">
            <a-collapse-panel key="1" header="Comics">

                <a-list :data-source="data.comics.items">
                  <a-list-item slot="renderItem" slot-scope="item, index" @click="comicModal(item, index)">
                    {{ item.name }}
                  </a-list-item>
                </a-list>

            </a-collapse-panel>
            <a-collapse-panel key="2" header="Series">

                <a-list :data-source="data.series.items">
                  <a-list-item slot="renderItem" slot-scope="item, index" >
                    {{ item.name }}
                  </a-list-item>
                </a-list>

            </a-collapse-panel>
            <a-collapse-panel key="3" header="Stories">

                <a-list :data-source="data.stories.items">
                  <a-list-item slot="renderItem" slot-scope="item, index">
                    {{ item.name }}
                  </a-list-item>
                </a-list>

            </a-collapse-panel>
            <a-collapse-panel key="4" header="Events">

                <a-list :data-source="data.events.items">
                  <a-list-item slot="renderItem" slot-scope="item, index">
                    {{ item.name }}
                  </a-list-item>
                </a-list>

            </a-collapse-panel>
          </a-collapse>
        </div>

      </div>


    </div>
  </div>
</template>

<script>
export default {
  name: "index",
  data() {
    return {
      results: null,
    };
  },
  created() {
    parseInt(this.$route.params.character_id);
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
      this.$axios
        .$get(
          "https://gateway.marvel.com/v1/public/characters/" + character_id,
          {
            params: {
              ts: 1,
              apikey: "78b447af6136f7b546b974a1461ecf61",
              hash: "5ace13da8a874e4080eff29c219043c3",
            },
          }
        )
        .then((response) => {
          console.log("response");
          console.log(response);
          this.results = response.data.results;
        });
    },

    comicModal(data, index){
      console.log("data")
      console.log(data)
      console.log("index")
      console.log(index)
      
    }

  },
};
</script>

<style scoped></style>
