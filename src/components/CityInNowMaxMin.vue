<template>
  <div class="maxMinIntro">
    <svg-icon class="icon" type="mdi" :path="mdiCalendarMonthPath"></svg-icon>
    <div class="content">5일간의 일기예보</div>
  </div>
  <div class="maxmin" v-for="forecast in cityInNowForecasts" :key="forecast">
    <div
      v-if="UTCtoKST(new Date(forecast.dt_txt)).substr(0, 10) > currentDate()"
    >
      <div
        class="date"
        v-if="UTCtoKST(new Date(forecast.dt_txt)).substr(11, 2) == '06'"
      >
        {{ UTCtoKST(new Date(forecast.dt_txt)).substr(5, 5) }}
      </div>
      <div
        class="min"
        v-if="UTCtoKST(new Date(forecast.dt_txt)).substr(11, 2) == '06'"
      >
        {{ (forecast.main.temp - 273.15).toFixed(1) }}°
      </div>
      <div
        class="bar"
        v-if="UTCtoKST(new Date(forecast.dt_txt)).substr(11, 2) == '06'"
      ></div>
      <div
        class="max"
        v-if="UTCtoKST(new Date(forecast.dt_txt)).substr(11, 2) == '15'"
      >
        {{ (forecast.main.temp - 273.15).toFixed(1) }}°
      </div>
    </div>
  </div>
</template>

<script>
import SvgIcon from "@jamescoyle/vue-icon";
import { mdiCalendarMonth } from "@mdi/js";

export default {
  name: "CityInNowMaxMin",
  props: {
    cityname: String,
    countryname: String,
  },
  data() {
    // 데이터 보관함
    return {
      cityInNowForecasts: [],
      mdiCalendarMonthPath: mdiCalendarMonth,
    };
  },
  created() {
    const current = new Date();

    let hour = current.getHours();
    if (hour < 10) hour = "0" + hour;

    const currentDateTime = this.currentDate() + " " + hour;

    fetch(
      "https://api.openweathermap.org/data/2.5/forecast?q=" +
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
        for (let i = 0; i < res.list.length; i++) {
          if (this.UTCtoKST(new Date(res.list[i].dt_txt)) > currentDateTime)
            this.cityInNowForecasts.push(res.list[i]);
        }
      })
      .catch((error) => {
        console.log(error);
      });
  },
  methods: {
    currentDate() {
      const current = new Date();

      const year = current.getFullYear(); // 년도
      let month = current.getMonth() + 1; // 월
      if (month < 10) month = "0" + month;
      let date = current.getDate(); // 날짜
      if (date < 10) date = "0" + date;
      const currentDay = year + "-" + month + "-" + date;

      return currentDay;
    },
    UTCtoKST(props) {
      return new Date(props.getTime() - 2 * (props.getTimezoneOffset() * 60000))
        .toISOString()
        .substring(0, 19);
    },
  },
  components: { SvgIcon },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.maxMinIntro {
  border-bottom: 1px solid white;
  height: 40px;
  color: white;
}

.maxMinIntro .icon {
  float: left;
}

.maxMinIntro .content {
  text-align: left;
  margin-left: 28px;
  margin-top: 2px;
  width: 130px;
  padding: 0px;
}

.maxmin {
  margin-bottom: 25px;
  font-size: 1.1rem;
  color: white;
}

.maxmin .date {
  float: left;
  width: 52px;
  margin-right: 17px;
}

.maxmin .min {
  float: left;
  width: 45px;
  margin-right: 5px;
  color: #d8d8d8;
}

.maxmin .bar {
  float: left;
  width: 160px;
  height: 10px;
  margin-top: 4px;
  border-radius: 10px;
  background-color: rgba(88, 88, 88, 0.6);
}
</style>
