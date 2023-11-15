<template>
  <div class="home">
    <div class="searchCity"><SearchCity /></div>
    <div class="cityInNowCurrent">
      <CityInNowCurrent cityname="Seoul" countryname="Korea" />
    </div>
    <div class="cityInNowForecasts">
      <CityInNowForecasts cityname="Seoul" countryname="Korea" />
    </div>
    <div class="additionalInfo">
      <div class="cityInNowMaxMin">
        <CityInNowMaxMin cityname="Seoul" countryname="Korea" />
      </div>
      <div class="cityInNowCurrentAdd">
        <CityInNowCurrentAdd cityname="Seoul" countryname="Korea" />
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

export default {
  name: "HomeView",
  data() {
    // 데이터 보관함
    return {
      daySkyImg: require("../assets/day_sky.jpg"),
      nightSkyImg: require("../assets/night_sky.jpg"),
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
  methods: {},
  components: {
    SearchCity,
    CityInNowCurrent,
    CityInNowMaxMin,
    CityInNowForecasts,
    CityInNowCurrentAdd,
  },
};
</script>

<style>
.home {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.home .background {
  position: absolute;
  top: 0px;
  left: 0px;
  width: 100%;
  height: auto;
  z-index: -1;
}

.home .searchCity {
  position: absolute;
  top: 22px;
  right: 2%;
}

.home .cityInNowCurrent {
  margin-top: 80px;
  margin-bottom: 60px;
  color: white;
}

.home .cityInNowForecasts {
  border-radius: 12px;
  background-color: rgba(63, 111, 157, 0.5);
  padding-left: 30px;
}

.home .additionalInfo {
  margin-top: 40px;
  border-radius: 12px;
  background-color: rgba(63, 111, 157, 0.5);
  width: 63%;
  padding: 20px;
}

.home .additionalInfo .cityInNowMaxMin {
  float: left;
  width: 340px;
  margin-top: 20px;
  margin-left: 30px;
}

.home .additionalInfo .cityInNowCurrentAdd {
  width: 300px;
  margin-left: 425px;
  list-style: none;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 15px;
}
</style>
