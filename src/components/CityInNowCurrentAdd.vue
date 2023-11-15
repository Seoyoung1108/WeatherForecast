<template>
  <div class="humidity" v-if="cityInNowCurrent[0]">
    <svg-icon class="icon" type="mdi" :path="mdiWaterPercentPath"></svg-icon>
    <div class="content">습도</div>
    <p>{{ cityInNowCurrent[0].main.humidity }}%</p>
  </div>
  <div class="feelslike" v-if="cityInNowCurrent[0]">
    <svg-icon
      class="icon"
      type="mdi"
      :path="mdiThermometerLinesPath"
    ></svg-icon>
    <div class="content">체감 온도</div>
    <p>{{ (cityInNowCurrent[0].main.feels_like - 273.15).toFixed(1) }}°</p>
  </div>
  <div class="wind" v-if="cityInNowCurrent[0]">
    <svg-icon class="icon" type="mdi" :path="mdiWeatherWindyPath"></svg-icon>
    <div class="content">바람</div>
    <img
      :style="`transform:rotate(${cityInNowCurrent[0].wind.deg}deg)`"
      alt="arrow"
      :src="arrowImg"
    />
    <div class="windspeed">
      {{ cityInNowCurrent[0].wind.speed.toFixed(1) }}
    </div>
    <span>m/s</span>
  </div>
  <div class="visibility" v-if="cityInNowCurrent[0]">
    <svg-icon class="icon" type="mdi" :path="mdiEyePath"></svg-icon>
    <div class="content">가시거리</div>
    <p>{{ (cityInNowCurrent[0].visibility / 100).toFixed(0) }}km</p>
  </div>
  <div class="rain" v-if="cityInNowCurrent[0]">
    <svg-icon class="icon" type="mdi" :path="mdiWeatherRainyPath"></svg-icon>
    <div class="content">강수량</div>
    <p>
      {{
        cityInNowCurrent[0].rain
          ? cityInNowCurrent[0].rain["1h"].toFixed(1)
          : 0
      }}mm
    </p>
  </div>
  <div class="pressure" v-if="cityInNowCurrent[0]">
    <svg-icon
      class="icon"
      type="mdi"
      :path="mdiArrowCollapseDownPath"
    ></svg-icon>
    <div class="content">기압</div>
    <div class="bar"></div>
    <div
      class="circle"
      :style="`left:${70 + cityInNowCurrent[0].main.pressure - 1012}px`"
    ></div>
    <span class="pressureValue">{{ cityInNowCurrent[0].main.pressure }}</span>
    <span class="unit">hPa</span>
  </div>
</template>

<script>
import SvgIcon from "@jamescoyle/vue-icon";
import {
  mdiWaterPercent,
  mdiEye,
  mdiThermometerLines,
  mdiWeatherWindy,
  mdiWeatherRainy,
  mdiArrowCollapseDown,
} from "@mdi/js";

export default {
  name: "CityInNowCurrentAdd",
  props: {
    cityname: String,
    countryname: String,
  },
  data() {
    // 데이터 보관함
    return {
      cityInNowCurrent: [],
      arrowImg: require("../assets/arrow.png"),
      mdiWaterPercentPath: mdiWaterPercent,
      mdiEyePath: mdiEye,
      mdiThermometerLinesPath: mdiThermometerLines,
      mdiWeatherWindyPath: mdiWeatherWindy,
      mdiWeatherRainyPath: mdiWeatherRainy,
      mdiArrowCollapseDownPath: mdiArrowCollapseDown,
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
  components: { SvgIcon },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div {
  width: 130px;
  height: 130px;
  background-color: #585858;
  color: white;
  text-align: left;
  border-radius: 10px;
  padding: 10px;
}

div .icon {
  float: left;
}
div .content {
  text-align: left;
  font-size: 0.9rem;
  margin-left: 26px;
  margin-top: 1px;
  width: 90px;
  height: 5px;
  padding: 0px;
}

div p {
  font-size: 2.7rem;
  text-align: center;
}

.wind {
  position: relative;
}

.wind img {
  position: absolute;
  width: 115px;
  top: 30px;
  left: 17px;
  z-index: 10;
  filter: invert(100%) sepia(100%) saturate(0%) hue-rotate(63deg)
    brightness(106%) contrast(101%);
}

.wind .windspeed {
  position: absolute;
  background-color: #585858;
  width: 25px;
  height: 25px;
  border-radius: 50%;
  text-align: center;
  line-height: 18px;
  top: 63px;
  left: 52px;
  z-index: 20;
}

.wind span {
  position: absolute;
  font-size: 0.5rem;
  z-index: 20;
  top: 90px;
  left: 66px;
}

.pressure {
  position: relative;
}

.pressure .bar {
  position: absolute;
  padding: 0;
  width: 110px;
  height: 10px;
  top: 47px;
  left: 20px;
  border-radius: 10px;
  background-color: rgba(216, 216, 216, 0.3);
}

.pressure .circle {
  position: absolute;
  padding: 0;
  top: 47px;
  width: 9px;
  height: 9px;
  border-radius: 50%;
  border: 1px solid #eaeaea;
  background-color: white;
}

.pressure .pressureValue {
  position: absolute;
  font-size: 2.3rem;
  top: 65px;
  left: 34px;
}

.pressure .unit {
  position: absolute;
  font-size: 0.9rem;
  top: 105px;
  left: 63px;
}
</style>
