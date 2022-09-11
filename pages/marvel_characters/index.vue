<template>
  <div>
    <div class="flex">
      <div class="wrapper grid grid-cols-4 gap-1 justify-items-center w-full">
        <div class="flex p-2.5" v-for="data in results">
          <a-card hoverable :loading="loading" style="width: 240px" @click="character_info(data)">
            <img
              slot="cover"
              :alt="data.name"
              :src="data.thumbnail.path + '.' + data.thumbnail.extension"
            />
            <a-card-meta>
              <template slot="title">
                  {{ data.name }}
              </template>

              <template slot="description">
                {{ data.desciption }}
              </template>
            </a-card-meta>
          </a-card>
        </div>

          <div class="col-start-4">
            <a-pagination
              v-model="current"
              :total="total"
              show-less-items
              show-size-changer
              :page-size-options="pageSizeOptions"
              :page-size="pageSize"
              @showSizeChange="onShowSizeChange"
              @change="onChange"
            />
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
      marvel_characters: null,
      loading: false,
      current: 1,
      total: null,
      pageSizeOptions: ['4', '8', '12', '16', '20'],
      pageSize: 4,
    };
  },
  created() {
    this.getData();
  },
  methods: {
    async getData() {
      this.loading = true;
      await this.$axios
        .$get("https://gateway.marvel.com/v1/public/characters", {
          params: {
            ts: 1,
            apikey: "78b447af6136f7b546b974a1461ecf61",
            hash: "5ace13da8a874e4080eff29c219043c3",
          },
        })
        .then((response) => {
          this.total = response.data.count;
          this.results = response.data.results;
          this.marvel_characters = this.results;
          this.loading = false;
        });
    },

    character_info(data){
      this.$router.push('/marvel_characters/' + data.id + '/view/');
    },

    onShowSizeChange(current, pageSize) {
      let characters = this.marvel_characters;
      let data = characters.slice((current - 1) * pageSize, current * pageSize);
      this.results = data;

      this.pageSize = pageSize;
    },

    onChange(pageNumber) {
      let characters = this.marvel_characters;
      let data = characters.slice((pageNumber - 1) * this.pageSize, pageNumber * this.pageSize);

      this.results = data;
    },

  },
};
</script>

<style scoped></style>
