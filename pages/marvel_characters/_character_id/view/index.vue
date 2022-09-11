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

    <div v-for="data in results" class="flex justify-center">
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
                <a-list-item
                  slot="renderItem"
                  slot-scope="item, index"
                  @click="comicModal(item, index, data)"
                >
                  {{ item.name }}
                </a-list-item>
              </a-list>
            </a-collapse-panel>
            <a-collapse-panel key="2" header="Series">
              <a-list :data-source="data.series.items">
                <a-list-item slot="renderItem" slot-scope="item, index">
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

      <a-modal :title="title" :visible="visible">
        <div v-for="data in comicInfo">{{ data.description }}</div>
        <template slot="footer">
          <a-button key="submit" type="primary" @click="handleOk">
            Ok
          </a-button>
        </template>
      </a-modal>
    </div>
  </div>
</template>

<script>
export default {
  name: "index",
  data() {
    return {
      results: null,
      title: null,
      visible: false,
      comicInfo: null,
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

    comicModal(data, index, result) {
      console.log("data");
      console.log(data);
      console.log("index");
      console.log(index);
      console.log("result");
      console.log(result);

      this.title = data.name;
      this.visible = true;

      this.getComicData(data);
    },

    getComicData(data) {
      this.$axios
        .$get(`${data.resourceURI}`, {
          params: {
            ts: 1,
            apikey: "78b447af6136f7b546b974a1461ecf61",
            hash: "5ace13da8a874e4080eff29c219043c3",
          },
        })
        .then((response) => {
          console.log("response");
          console.log(response);
          this.comicInfo = response.data.results;
        });
    },

    handleOk() {
      this.visible = false;
    },
  },
};
</script>

<style scoped></style>
