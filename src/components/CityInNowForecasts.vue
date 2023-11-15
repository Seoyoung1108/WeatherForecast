<template>
  <div
    class="forecast"
    v-for="forecast in cityInNowForecasts.slice(0, 13)"
    :key="forecast"
  >
    <p v-if="forecast.dt_txt">
      {{ dayForecast(forecast.dt_txt) }}
    </p>
    <p v-if="forecast.dt_txt">
      {{ (forecast.main.temp - 273.15).toFixed(1) }}°C
    </p>
  </div>
</template>

<script>
export default {
  name: "CityInNowForecasts",
  props: {
    cityname: String,
    countryname: String,
  },
  data() {
    // 데이터 보관함
    return {
      cityInNowForecasts: [],
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
    dayForecast(props) {
      const day = new Date(
        new Date(props).setHours(new Date(props).getHours() + 9)
      );
      const today = new Date();

      if (
        today.toString().substring(0, 15) === day.toString().substring(0, 15)
      ) {
        return "오늘 " + this.UTCtoKST(new Date(props)).substr(11, 2) + "시";
      } else if (
        new Date(today.setDate(today.getDate() + 1))
          .toString()
          .substring(0, 15) == day.toString().substring(0, 15)
      ) {
        return "내일 " + this.UTCtoKST(new Date(props)).substr(11, 2) + "시";
      } else {
        return "모레 " + this.UTCtoKST(new Date(props)).substr(11, 2) + "시";
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.forecast {
  float: left;
  margin-right: 30px;
  color: white;
}
</style>
