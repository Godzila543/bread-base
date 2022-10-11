<template>
  <v-card class="mx-auto">
    <v-img
      :src="content.img"
      height="200px"
      cover
      gradient="to bottom, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.4)"
      class="align-end text-h0"
      ><v-card-title style="color: white">{{ content.title }}</v-card-title>
    </v-img>

    <v-card-subtitle class="mt-1 pl-1">
      {{ content.date }}
    </v-card-subtitle>
    <v-card-text class="pt-0">
      <v-chip-group disabled>
        <v-chip
          v-for="tag in content.tags"
          :key="tag"
          class="ma-1"
          :label="filterTags.includes(tag)"
          :color="filterTags.includes(tag) ? 'primary' : ''"
        >
          {{ tag }}
        </v-chip>
      </v-chip-group>
      <div
        class="text-subtitle-1"
        v-for="(line, i) in content.body.split('\n').slice(0, 4)"
        :key="i"
        :style="{ opacity: (4.5 - i) / 6 }"
      >
        {{ line }}
      </div>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  name: "Recipe",
  props: ["content", "filterTags"],
  methods: {
    sortTags() {
      this.content.tags.sort((a, b) => {
        const ai = this.filterTags.includes(a) ? 1 : 0;
        const bi = this.filterTags.includes(b) ? 1 : 0;
        return bi - ai;
      });
    },
  },
  watch: {
    filterTags: {
      handler() {
        this.sortTags();
      },
      deep: true,
    },
  },
};
</script>

<style>
</style>