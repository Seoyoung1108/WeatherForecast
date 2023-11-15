<template>
  <div class="name" v-if="cityInNowCurrent[0]">
    {{ cityInNowCurrent[0].name }}
  </div>
  <div class="temperature" v-if="cityInNowCurrent[0]">
    {{ (cityInNowCurrent[0].main.temp - 273.15).toFixed(1) }}°C
  </div>
  <div class="weather" v-if="cityInNowCurrent[0]">
    {{ cityInNowCurrent[0].weather[0].main }},
    {{ cityInNowCurrent[0].weather[0].description }}
  </div>
</template>

<script>
export default {
  name: "CityInNowCurrent",
  props: {
    cityname: String,
    countryname: String,
  },
  data() {
    // 데이터 보관함
    return {
      cityInNowCurrent: [],
    };
  },
  created() {
    fetch(
      "https://api.openweathermap.org/data/2.5/weather?q=" +
        this.cityname +
        "," +
        this.countryname +
        "&units=&lang=kr&appid=" +
        process.env.VUE_APP_API_KEY
    )
      .then((response) => {
        return response.json();
      })
      .then((res) => {
        this.cityInNowCurrent.push(res);
      })
      .catch((error) => {
        console.log(error);
      });
  },
  methods: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.name {
  font-size: 2.8rem;
}

.temperature {
  font-size: 4.2rem;
}

.weather {
  font-size: 1.3rem;
}
</style>
