<template>
  <v-app :theme="light ? 'light' : 'dark'">
    <v-app-bar app color="primary" permanent>
      <v-app-bar-title>Bread Base (The Bread Database)</v-app-bar-title>
      <template v-slot:append>
        <v-btn icon="mdi-theme-light-dark" @click="light = !light"></v-btn>
      </template>
    </v-app-bar>
    <v-navigation-drawer rail expand-on-hover color="accent">
      <v-list nav color="primary">
        <v-list-item
          :active="tab == 'recipes'"
          prepend-icon="mdi-baguette"
          title="Recipes"
          @click="tab = 'recipes'"
        />
        <v-list-item
          :active="tab == 'logeditor'"
          prepend-icon="mdi-notebook"
          title="Add Log"
          @click="tab = 'logeditor'"
        />
      </v-list>
    </v-navigation-drawer>
    <v-main>
      <v-row style="height: 100%" v-if="tab == 'recipes'">
        <Drawer
          :tags="allTags"
          @selected="(newTags) => (filterTags = newTags)"
          @content="(text) => (content = text)"
          fill-height
        ></Drawer>

        <CardArea :tags="filterTags" :content="content" :recipes="recipes" />
      </v-row>
      <v-row v-if="tab == 'logeditor'" justify="center">
        <LogEditor
          :tags="allTags"
          @createRecipe="createRecipe"
          @createTag="createTag"
        />
      </v-row>
    </v-main>
  </v-app>
</template>

<script>
const BASE_KEY = "breadbase-base";
const TAGS_KEY = "breadbase-tags";

import HelloWorld from "./components/HelloWorld.vue";
import Drawer from "./components/Drawer.vue";
import CardArea from "./components/CardArea.vue";
import LogEditor from "./components/LogEditor.vue";

export default {
  name: "App",

  components: {
    HelloWorld,
    Drawer,
    CardArea,
    LogEditor,
  },

  data: () => ({
    filterTags: [],
    allTags: JSON.parse(localStorage.getItem(TAGS_KEY) || "[]"),
    content: "",
    light: true,
    tab: "recipes",
    recipes: JSON.parse(localStorage.getItem(BASE_KEY) || "[]"),
    //
  }),
  methods: {
    createRecipe(r) {
      this.recipes.push(r);
      this.tab = "recipes";
      localStorage.setItem(BASE_KEY, JSON.stringify(this.recipes));
    },
    createTag(t) {
      this.allTags.push(t);
      localStorage.setItem(TAGS_KEY, JSON.stringify(this.allTags));
    },
  },
};
</script>
<style>
html {
  overflow-y: auto !important;
}
</style>