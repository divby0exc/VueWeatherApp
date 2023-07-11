<template>
  <v-responsive class="mx-auto" max-width="300">
    <v-text-field
      append-inner-icon="mdi-search"
      clearable
      label="Search city"
      hide-details="auto"
      hint="Press outside the searchbar to search"
      type="text"
      id="search"
      v-model="city"
      @blur="searchCity"
    ></v-text-field>
  </v-responsive>
</template>

<script lang="ts">
export default {
  data() {
    return {
      city: "",
    };
  },
  methods: {
    searchCity() {
      if(this.city.length > 1) {
        this.$emit("city-searched", this.city);
      }
      this.city = '';
    },
  },
};
</script>
