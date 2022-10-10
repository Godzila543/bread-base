<template>
  <v-row>
    <v-col>
      <v-text-field
        label="Search Tags"
        v-model="tagSearch"
        prepend-inner-icon="mdi-filter"
        variant="outlined"
        color="primary"
        @change="searchEnter"
        clearable
      ></v-text-field>
    </v-col>
  </v-row>
  <template v-for="tag in tags" :key="tag">
    <v-chip
      :variant="selectedTags.includes(tag) ? 'tonal' : 'tonal'"
      :color="selectedTags.includes(tag) ? 'primary' : 'accent'"
      class="ma-1"
      v-if="tag.toLowerCase().includes(tagSearch.toLowerCase())"
      @click="clickChip(tag)"
    >
      {{ tag }}
    </v-chip>
  </template>
  <v-chip
    @click="clickChip(tagSearch)"
    color="success"
    variant="tonal"
    v-if="
      !tags.some((t) => t.toLowerCase().includes(tagSearch.toLowerCase())) &&
      editor
    "
    >Create Tag "{{ tagSearch }}"
  </v-chip>
</template>

<script>
export default {
  name: "TagSearch",
  props: { tags: Array, editor: Boolean },
  emits: ["selected", "create"],
  data: () => ({
    selectedTags: [],
    tagSearch: "",
  }),
  methods: {
    clickChip(tag) {
      if (this.selectedTags.includes(tag))
        this.selectedTags.splice(
          this.selectedTags.findIndex((t) => t == tag),
          1
        );
      else if (this.tags.includes(tag)) this.selectedTags.push(tag);
      else {
        const index = this.tags.findIndex((t) =>
          t.toLowerCase().includes(tag.toLowerCase())
        );
        if (index != -1) this.selectedTags.push(this.tags[index]);
        else if (this.editor) this.createTag(tag);
      }

      this.$emit("selected", this.selectedTags);
      // this.sortTags();
    },
    sortTags() {
      this.tags.sort((a, b) => {
        const ai = this.selectedTags.includes(a) ? 1 : 0;
        const bi = this.selectedTags.includes(b) ? 1 : 0;
        return bi - ai;
      });
    },
    createTag(tag) {
      this.$emit("create", tag);
      this.selectedTags.push(tag);
    },
    searchEnter() {
      console.log("sdfsdf");
      this.clickChip(this.tagSearch);
      this.tagSearch = "";
    },
  },
};
</script>

<style>
</style>