<template>
  <v-app>
    <v-header>
      <Headline />
    </v-header>
    <v-main>
      <CityButton v-bind:savedCities="savedCities" />

      <SearchField @city-searched="fetchCityWeather" />
      <p>{{ errorMsg }}</p>
      <CurrentWeatherBox
        :temperature="savedData.temperature"
        :icon="savedData.icon"
        :pressure="savedData.pressure"
        :windspeed="savedData.windspeed"
        :humidity="savedData.humidity"
        :description="savedData.description"
        :time="savedData.time"
        :name="savedData.name"
      />

      <h3 class="text-center">Hourly forecast</h3>
      <HourlyWeatherBox v-bind:hourly="savedData.hourly" />
    </v-main>
  </v-app>
</template>

<script>
import Headline from "@/components/Headline.vue";
import SearchField from "@/components/SearchField.vue";
import CurrentWeatherBox from "@/components/CurrentWeatherBox.vue";
import HourlyWeatherBox from "@/components/HourlyWeatherBox.vue";
import CityButton from "./components/CityButton.vue";
import axios from "axios";

export default {
  components: {
    Headline,
    SearchField,
    CurrentWeatherBox,
    HourlyWeatherBox,
    CityButton,
  },
  data() {
    return {
      savedData: {
        temperature: null,
        icon: "",
        pressure: null,
        windspeed: null,
        humidity: null,
        description: null,
        time: null,
        name: null,
        hourly: {
          hourlyIcon: [],
          hourlyTemp: [],
          hourlyTime: [],
        },
      },
      errorMsg: "",
    };
  },
  methods: {
    async fetchCityWeather(city) {
      try {
        // Trying to start with Geo locations due to issues with fetching
        const GeoCoord = await axios.get(
          " http://api.openweathermap.org/geo/1.0/direct?q=" +
            city +
            "&limit=1&appid=" +
            "INSERT APP KEY HERE"
        );
        const GeoCoordRes = GeoCoord.data;
        const lat = GeoCoordRes[0].lat;
        const lon = GeoCoordRes[0].lon;

        // Fetch the weather data based on the geo location
        const WeatherData = await axios.get(
          "https://api.openweathermap.org/data/2.5/weather?lat=" +
            lat +
            "&lon=" +
            lon +
            "&appid=" +
            "INSERT APP KEY HERE"
        );
        const WeatherDataRes = WeatherData.data;
        const iconCode = WeatherDataRes.weather[0].icon;

        // Time to try this way and save all the info again
        const iconUrl =
          "https://openweathermap.org/img/wn/" + iconCode + "@2x.png";
        this.savedData.temperature = (WeatherDataRes.main.temp - 273.15).toFixed(0);
        this.savedData.pressure = WeatherDataRes.main.pressure;
        this.savedData.windspeed = WeatherDataRes.wind.speed;
        this.savedData.humidity = WeatherDataRes.main.humidity;
        this.savedData.description = WeatherDataRes.weather[0].description;
        this.savedData.icon = iconUrl;
        this.savedData.name = WeatherDataRes.name;

        // Hourly Forecast API
        const HourlyWeatherForecast = await axios.get(
          "https://api.openweathermap.org/data/2.5/forecast?lat=" +
            lat +
            "&lon=" +
            lon +
            "&appid=" +
            "INSERT APP KEY HERE"
        );
        const HourlyWeatherData = HourlyWeatherForecast.data;

        this.savedData.hourly.hourlyIcon = [];
        this.savedData.hourly.hourlyTemp = [];
        this.savedData.hourly.hourlyTime = [];

        for (let i = 0; i < 6; i++) {
          let timeArr = HourlyWeatherData.list[i].dt_txt.split(" ");

          this.savedData.hourly.hourlyIcon.push(
            "https://openweathermap.org/img/wn/" +
              HourlyWeatherData.list[i].weather[0].icon +
              "@2x.png"
          );
          this.savedData.hourly.hourlyTemp.push(
            (HourlyWeatherData.list[i].main.temp - 273.15).toFixed(0)
          );
          this.savedData.hourly.hourlyTime.push(timeArr[1].substring(0, 5));
        }
        // let timeArr = HourlyWeatherData.list[0].dt_txt.split(" ");
        //   console.log(timeArr[1].substring(0,5))
        // this.hourlyIcon =
        //   "https://openweathermap.org/img/wn/" +
        //   HourlyWeatherData.list[0].weather[0].icon +
        //   "@2x.png";

        // this.hourlyTemp = (HourlyWeatherData.list[0].main.temp - 273,
        // 15).toFixed(0);

        // this.hourlyTime = timeArr[1];
      } catch (error) {
        this.errorMsg = error;
      }
    },
  },
};
</script>
