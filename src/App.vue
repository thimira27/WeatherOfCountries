<template>
  <!-- Apply the 'warm' class if weather.main is defined and temperature is greater than 16 -->
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : '' 
    ">
    <main>
      <!-- Bind the input value to the 'query' data property -->
      <!-- Call the 'fetchWeather' method when a key is pressed -->
      <div class="search_box">
        <input
          type="text"
          class="search_bar"
          placeholder="search..."
          v-model="query" 
          @keypress="fetchWeather" 
        />
      </div>

      <!-- Render the weather information if 'weather.main' is defined -->
      <div class="weather_wrap" v-if="typeof weather.main != 'undefined'"> 
        <div class="location_box">
          <!-- Display the location and country -->
          <div class="location">
            {{ weather.name }} , {{ weather.sys.country }} 
          </div>
          <!-- Display the formatted date -->
          <div class="date">{{ dateBuilder() }}</div> 
        </div>
        <div class="weather_box">
          <!-- Display the rounded temperature -->
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div> 
          <!-- Display the weather description -->
          <div class="weather">{{ weather.weather[0].main }}</div> 
        </div>
      </div>

      <!-- Render the footer if 'weather.main' is defined -->
      <footer class="undFooter" v-if="typeof weather.main != 'undefined'"> 
        <p>&copy; 2023 : Thimira Galahitiyawa | Project <b>WeatherOfCountries</b></p>
      </footer>

      <!-- Render the alternative footer if 'weather.main' is undefined -->
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
      api_key: "a1a4b330e33117ff284a43862ae6faba", // API key for OpenWeatherMap API
      url_base: "https://api.openweathermap.org/data/2.5/", // Base URL for OpenWeatherMap API
      query: "", // Stores the value of the search input
      weather: {}, // Stores the fetched weather data
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") { // Check if the Enter key was pressed
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}` // Construct the API URL with the search query and API key
        )
          .then((res) => {
            return res.json(); // Convert the response to JSON
          })
          .then(this.setResult); // Call the 'setResult' method with the fetched weather data
      }
    },

    setResult(results) {
      this.weather = results; // Update the 'weather' data property with the fetched weather data
      console.log(this.weather.weather[0].main); // Log the main weather description to the console
    },

    dateBuilder() {
      let d = new Date(); // Create a new Date object
      let options = {
        weekday: "long",
        month: "long",
        day: "numeric",
        year: "numeric",
      };
      return d.toLocaleDateString(undefined, options); // Format the date using the specified options and return the formatted date
    },
  },
};
</script>

//all styling parts under here
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
