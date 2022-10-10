<template>
  <v-col
    xl="2"
    lg="3"
    class="mt-2 pr-0 mr-0 d-flex flex-column justify-space-between"
  >
    <v-card flat color="background">
      <!-- <v-card-title>Filtering</v-card-title> -->
      <v-card-text>
        <v-text-field
          color="primary"
          variant="outlined"
          label="Content"
          class="mb-0 pb-0"
          v-model="contentSearch"
          @update:model-value="$emit('content', contentSearch.toLowerCase())"
          prepend-inner-icon="mdi-magnify"
          clearable
        ></v-text-field>

        <TagSearch
          :tags="tags"
          @selected="(selected) => $emit('selected', selected)"
        />
      </v-card-text>
    </v-card>
  </v-col>
</template>

<script>
import TagSearch from "./TagSearch.vue";

export default {
  components: { TagSearch },
  props: ["tags"],
  name: "Drawer",
  data: () => ({
    contentSearch: "",
  }),
  methods: {},
};
</script>

<style>
</style>