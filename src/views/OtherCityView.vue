<template>
  <div class="home">
    <div class="searchCity"><SearchCity /></div>
    <div class="cityInNowCurrent">
      <CityInNowCurrent
        :cityname="this.$route.params.cityname"
        :countryname="this.$route.params.countryname"
      />
    </div>
    <div class="cityInNowForecasts">
      <CityInNowForecasts
        :cityname="this.$route.params.cityname"
        :countryname="this.$route.params.countryname"
      />
    </div>
    <div class="additionalInfo">
      <div class="cityInNowMaxMin">
        <CityInNowMaxMin
          :cityname="this.$route.params.cityname"
          :countryname="this.$route.params.countryname"
        />
      </div>
      <div class="cityInNowCurrentAdd">
        <CityInNowCurrentAdd
          :cityname="this.$route.params.cityname"
          :countryname="this.$route.params.countryname"
        />
      </div>
    </div>
    <img
      v-if="new Date().getHours() >= 6 && new Date().getHours() < 18"
      class="background"
      alt="daySky"
      :src="daySkyImg"
    />
    <img v-else class="background" alt="daySky" :src="nightSkyImg" />
  </div>
</template>

<script>
import SearchCity from "../components/SearchCity.vue";
import CityInNowCurrent from "../components/CityInNowCurrent.vue";
import CityInNowForecasts from "../components/CityInNowForecasts.vue";
import CityInNowMaxMin from "../components/CityInNowMaxMin.vue";
import CityInNowCurrentAdd from "../components/CityInNowCurrentAdd.vue";
import cities from "../assets/cities";

export default {
  name: "OtherCityView",
  data() {
    // 데이터 보관함
    return {
      daySkyImg: require("../assets/day_sky.jpg"),
      nightSkyImg: require("../assets/night_sky.jpg"),
      cities: cities,
      cityname: "seoul",
    };
  },
  created() {
    fetch(
      "https://api.openweathermap.org/data/2.5/weather?q=seoul,Korea&units=&lang=kr&appid=" +
        process.env.VUE_APP_API_KEY
    )
      .then((response) => {
        return response.json();
      })
      .then((res) => {
        this.cityInNowCurrent = res;
      })
      .catch((error) => {
        console.log(error);
      });
  },
  methods: {
    submitForm() {
      console.log(this.cityname);
      this.$router.push({ name: "about", props: { cityname: this.cityname } });
    },
    gotoAnotherCity() {},
  },
  components: {
    SearchCity,
    CityInNowCurrent,
    CityInNowMaxMin,
    CityInNowForecasts,
    CityInNowCurrentAdd,
  },
};
</script>

<style></style>
