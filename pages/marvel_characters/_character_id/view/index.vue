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


        <div class="flex flex-col text-center text-3xl">
          {{ data.name }}
        </div>

        <div>
          <a-collapse accordion>
            <a-collapse-panel key="1" header="Comics">
              <div v-for="data in results">
                <p v-for="comic in data.comics.items">
                  {{comic.name}}
                </p>
              </div>
            </a-collapse-panel>
            <a-collapse-panel key="2" header="Series">
              <div v-for="data in results">
                <p v-for="comic in data.series.items">
                  {{comic.name}}
                </p>
              </div>
            </a-collapse-panel>
            <a-collapse-panel key="3" header="Stories">
              <div v-for="data in results">
                <p v-for="comic in data.stories.items">
                  {{comic.name}}
                </p>
              </div>
            </a-collapse-panel>
            <a-collapse-panel key="4" header="Events">
              <div v-for="data in results">
                <p v-for="comic in data.events.items">
                  {{comic.name}}
                </p>
              </div>
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
  },
};
</script>

<style scoped></style>
