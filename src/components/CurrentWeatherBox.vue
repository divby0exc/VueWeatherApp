<template>
  <v-card>
    <v-card-item align="center">
      <v-row align="center" justify="center">
        <v-col cols="auto">
          <v-card-title>{{ name }} <v-img :src="icon"></v-img> </v-card-title>
        </v-col>
        <v-col cols="auto">
          <v-card-title>Temp: {{ temperature }}°</v-card-title>
        </v-col>
      </v-row>

      <v-card-subtitle>{{ description }}</v-card-subtitle>

      <v-card-text>Humidity: {{ humidity }}%</v-card-text>
      <v-card-text>Pressure: {{ pressure }} hPa </v-card-text>
      <v-card-text>Windspeed: {{ windspeed }} m/s</v-card-text>
    </v-card-item>
  </v-card>
</template>

<script lang="ts">
export default {
  props: [
    "icon",
    "temperature",
    "description",
    "humidity",
    "pressure",
    "windspeed",
    "name",
  ],
  data() {
    return {};
  },
};
</script>
