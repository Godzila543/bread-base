<template>
  <v-col cols="7">
    <v-container>
      <v-card variant="outlined" flat color="accent">
        <v-card-title>Create a New Entry</v-card-title>
        <v-card-text>
          <v-row>
            <v-col cols="8">
              <v-text-field label="Title" variant="outlined" v-model="title" />
              <v-textarea
                rows="15"
                label="Body"
                variant="outlined"
                v-model="body"
              />
            </v-col>
            <v-col>
              <v-text-field
                label="Image Url"
                variant="outlined"
                v-model="imgUrl"
              />
              <v-text-field
                label="Date (yyyy-mm-dd)"
                variant="outlined"
                v-model="date"
                :placeholder="today()"
              />
              <TagSearch
                :tags="tags"
                editor
                @selected="(selected) => (chosenTags = selected)"
                @create="(t) => $emit('createTag', t)"
              />
            </v-col>
          </v-row>

          <v-btn block color="success" variant="tonal" @click="create"
            >Create Entry</v-btn
          >
        </v-card-text>
      </v-card>
    </v-container>
  </v-col>
</template>

<script>
import TagSearch from "./TagSearch.vue";

export default {
  name: "LogEditor",
  components: { TagSearch },
  props: ["tags"],
  data: () => ({
    title: "",
    body: "",
    chosenTags: [],
    imgUrl: "",
    date: "",
  }),
  created() {
    this.date = this.today();
  },
  methods: {
    create() {
      const newRecipe = {
        title: this.title,
        body: this.body,
        tags: this.chosenTags,
        img: this.imgUrl,
        date: this.date,
      };
      this.$emit("createRecipe", newRecipe);
    },
    today() {
      var today = new Date();
      var dd = String(today.getDate()).padStart(2, "0");
      var mm = String(today.getMonth() + 1).padStart(2, "0"); //January is 0!
      var yyyy = today.getFullYear();

      return yyyy + "-" + mm + "-" + dd;
    },
  },
};
</script>

<style>
</style>