<template>
  <v-col class="mt-2 mr-2 ml-0 pl-0">
    <v-container>
      <v-row>
        <template v-for="(recipe, i) in recipes" :key="recipe.title">
          <v-col
            cols="6"
            lg="4"
            xl="3"
            v-if="
              tags.every((t) => recipe.tags.includes(t)) &&
              (recipe.title.toLowerCase().includes(content) ||
                recipe.body.toLowerCase().includes(content))
            "
          >
            <Recipe
              @click="expandCard(i)"
              :content="recipe"
              :filterTags="tags"
            />
          </v-col>
        </template>
      </v-row>
    </v-container>
  </v-col>
  <v-dialog v-model="cardDialog" temporary max-width="60%">
    <Recipe
      style="width: 100%"
      :content="recipes[cardIndex]"
      expanded
      :filterTags="tags"
    />
  </v-dialog>
</template>

<script>
import Recipe from "./Recipe.vue";

export default {
  name: "CardArea",
  components: { Recipe },
  props: ["recipes", "tags", "content"],
  data: () => ({
    cardIndex: 0,
    cardDialog: false,
  }),
  methods: {
    expandCard(index) {
      this.cardIndex = index;
      this.cardDialog = true;
    },
  },
};
</script>

<style>
</style>