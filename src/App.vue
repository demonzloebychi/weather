<script>
export default {
  data() {
    return {
      location: "",
      temp: 0,
      description: "",
      loading: false,
      error: false,
      searchQuery: "",
    };
  },
  computed: {
    weatherClass() {
      if (this.description.includes("Sunny")) {
        return "sunny";
      } else if (this.description.includes("Overcast")) {
        return "overcast";
      } else if (this.description.includes("Partly cloudy")) {
        return "cloudy";
      } else {
        return "";
      }
    },
  },

  methods: {
    weatherSearch() {
      this.loading = true;
      this.error = false;
      fetch(
        `https://api.weatherapi.com/v1/current.json?key=8b46cdee7f8c4e39a33102019241105&q=${this.searchQuery}`
      )
        .then((response) => response.json())
        .then((data) => {
          this.loading = false;
          this.location = data.location.name;
          this.temp = data.current.temp_c;
          this.description = data.current.condition.text;
          this.resetSearchQuery();
        })
        .catch((error) => {
          this.loading = false;
          this.error = true;
          console.error(error);
        });
    },
    resetSearchQuery() {
      this.searchQuery = " ";
    },
  },
};
</script>

<template>
  <div class="weather" :class="weatherClass">
    <div class="container">
      <div class="card weather-form">
        <input
          type="text"
          class="weather-form__input"
          placeholder="Enter city"
          v-model="searchQuery"
          @keyup.enter="weatherSearch()"
        />
        <button class="weather-form__btn" @click="weatherSearch">Enter</button>
      </div>

      <div class="card weather-load" v-if="loading">Loading...</div>

      <div
        class="weather-info"
        v-show="!error && location && temp !== 0 && description"
      >
        <div class="card" v-if="error">Error</div>

        <div class="weather-info__text">
          <p class="card">{{ location }}</p>
          <p class="card">{{ temp }}°C</p>
          <p class="card">{{ description }}</p>
        </div>
      </div>
    </div>

    <div class="weather-bg">
      <div class="">
        <img src="./assets/rainy.png" alt="" class="weather-bg__img bg" />

        <img
          src="./assets/overcast.jpg"
          alt=""
          class="weather-bg__img overcast"
        />

        <img src="./assets/cloudy.jpg" alt="" class="weather-bg__img cloudy" />

        <img src="./assets/sunny.jpg" alt="" class="weather-bg__img sunny" />
      </div>
    </div>
  </div>
</template>

<style scoped></style>
