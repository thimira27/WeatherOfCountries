<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''
    "
  >
    <main>
      <div class="search_box">
        <input
          type="text"
          class="search_bar"
          placeholder="search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather_wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location_box">
          <div class="location">
            {{ weather.name }} , {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather_box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>

      <footer class="undFooter" v-if="typeof weather.main != 'undefined'">
        <p>&copy; 2023 : Thimira Galahitiyawa | Project <b>WeatherOfCountries</b></p>
      </footer>
      <footer class="NotundFooter" v-if="typeof weather.main == 'undefined'">
        <p>&copy; 2023 : Thimira Galahitiyawa | Project <b>WeatherOfCountries</b></p>
      </footer>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "a1a4b330e33117ff284a43862ae6faba",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResult);
      }
    },

    setResult(results) {
      this.weather = results;
      console.log(this.weather.weather[0].main);
    },

    dateBuilder() {
      let d = new Date();
      let options = {
        weekday: "long",
        month: "long",
        day: "numeric",
        year: "numeric",
      };
      return d.toLocaleDateString(undefined, options);
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Courier New", Courier, monospace;
}

#app {
  background-image: url("./assets/misty-mountains-4k-hr-1536x864.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;

}

#app.warm {
  background-image: url("./assets/dragon-of-pink-mountains-minimal-5k-84.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.2),
    rgba(0, 0, 0, 0.8)
  );
}

.search_box {
  width: 50%;
  margin-bottom: 30px;
  margin-left: 25%;
}

.search_box .search_bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 0px 16px 0px 16px;
}

.search_box .search_bar:focus {
  box-shadow: 0px 0px 25px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 16px 0px 16px 0px;
}

.location_box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location_box .date {
  color: #fff;
  font-size: 22px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}

.weather_box {
  text-align: center;
}

.weather_box .temp {
  color: #fff;
  font-size: 96px;
  font-weight: 800;
  display: inline-block;
  padding: 10px 25px;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather_box .weather {
  font-size: 40px;
  color: #fff;
  font-weight: 400;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.undFooter {
  color: #fff;
  margin-top: 14%;
  margin-left: 58%;
}

.NotundFooter {
  color: #fff;
  margin-top: 38%;
  margin-left: 58%;
}

</style>
