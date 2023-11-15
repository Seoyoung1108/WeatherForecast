<template>
  <div @click="doClick()" class="searchBox">
    <input type="text" v-model="id" /><svg-icon
      v-if="toggle"
      class="icon"
      type="mdi"
      :path="mdiMenuUpPath"
    ></svg-icon>
    <svg-icon v-else class="icon" type="mdi" :path="mdiMenuDownPath"></svg-icon>
  </div>
  <div class="cities" v-if="toggle">
    <div class="city" v-for="city in cities" :key="city">
      <router-link
        class="go"
        @mouseover="doMouseOver(city.cityname)"
        :to="`/${city.cityname}/${city.countryname}`"
        ><div class="name">{{ city.cityname }}, {{ city.countryname }}</div>
        <svg-icon
          class="icon"
          type="mdi"
          :path="mdiArrowTopRightThickPath"
        ></svg-icon>
      </router-link>
    </div>
  </div>
</template>

<script>
import SvgIcon from "@jamescoyle/vue-icon";
import { mdiArrowTopRightThick, mdiMenuDown, mdiMenuUp } from "@mdi/js";
import cities from "../assets/cities";

export default {
  name: "SearchCity",
  data() {
    // 데이터 보관함
    return {
      mdiArrowTopRightThickPath: mdiArrowTopRightThick,
      mdiMenuDownPath: mdiMenuDown,
      mdiMenuUpPath: mdiMenuUp,
      cities: cities,
      id: "",
      toggle: false,
    };
  },
  methods: {
    doClick() {
      this.toggle = !this.toggle;
    },
    doMouseOver(props) {
      this.id = props;
    },
  },
  components: {
    SvgIcon,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.searchBox {
  width: 200px;
  height: 26px;
  background-color: white;
  cursor: pointer;
  border-radius: 8px;
  border: 1px solid #eaeaea;
}

.searchBox input {
  margin-top: 4px;
  margin-left: 7px;
  width: 155px;
  float: left;
  border: none;
  outline: none;
}

.searchBox .icon {
  margin-top: 1px;
}

.cities {
  width: 200px;
  height: 200px;
  border-radius: 10px;
  border: 1px solid #bdbdbd;
  background-color: white;
  overflow: auto;
}

.cities .city {
  height: auto;
  color: black;
}

.cities .city .go .name {
  width: 140px;
  font-size: 0.9rem;
  margin-top: 5px;
  margin-left: 9px;
  float: left;
  text-align: left;
  text-decoration: none;
  color: black;
}

.cities .city .go .icon {
  margin-top: 3px;
  color: black;
}
</style>
