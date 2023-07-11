<template>
  <v-card class="mx-auto" max-width="auto">
    <v-card-item >
      <v-row align="center" justify="center">
        <v-col cols="auto" md="4">
          <v-card-title
            ><v-img
              v-for="(icon, index) in this.hourly.hourlyIcon"
              :src="icon"
              :key="index"
            ></v-img>
            {{ icon }}</v-card-title
          >

          <v-card-title
            v-for="(temp, index) in this.hourly.hourlyTemp"
            :key="index"
          >
            {{ temp }}
          </v-card-title>

          <v-card-title
            v-for="(time, index) in this.hourly.hourlyTime"
            :key="index"
          >
            {{ time }}
          </v-card-title>
        </v-col>
        <!-- <v-card-text v-if="hourlyIcon">{{ hourlyIcon }}</v-card-text>
        <v-card-text v-if="hourlyTemp">{{ hourlyTemp }}</v-card-text>
        <v-card-text v-if="hourlyTime">{{ hourlyTime }}</v-card-text> -->
      </v-row>
    </v-card-item>
  </v-card>
</template>
<script>
export default {
  props: ["hourly"],
  data() {
    return {};
  },
};
</script>
