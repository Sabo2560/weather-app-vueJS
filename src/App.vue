<template>
  <div id="app" :class="getClass">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="City, Country ..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="default" v-if="!weather.location">
        <h2>Where is your current location?</h2>
        <img src="./assets/default1-bg.png" class="rounded mx-auto d-block" alt="image" />
      </div>

      <div class="weather-wrap" v-if="weather.location">
        <div class="location-box">
          <div class="location">{{ getLocation }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ getTemperature }}°C</div>
          <div class="weather">{{ getCondition }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      api_key: 'cb5b85e8bbad4355b06143637232210',
      url_base: 'https://api.weatherapi.com/v1/',
      query: '',
      weather: {}
    };
  },
  methods: {
    async fetchWeather(e) {
      if (e.key === 'Enter') {
        try {
          const res = await fetch(
            `${this.url_base}current.json?q=${this.query}&key=${this.api_key}&aqi=no`
          );
          const data = await res.json();
          this.setResults(data);
        } catch (error) {
          console.error('Error fetching weather data', error);
        }
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        'January',
        'February',
        'March',
        'April',
        'May',
        'June',
        'July',
        'August',
        'September',
        'October',
        'November',
        'December'
      ];
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  },
  computed: {
    getClass() {
      if (!this.weather.location) return 'default-background';
      return this.weather.current.temp_c > 16 ? 'warm' : 'cold';
    },
    getLocation() {
      if (this.weather.location) {
        return `${this.weather.location.name}, ${this.weather.location.country}`;
      }
      return '';
    },
    getTemperature() {
      if (this.weather.location) {
        return Math.round(this.weather.current.temp_c);
      }
      return '';
    },
    getCondition() {
      if (this.weather.location) {
        return this.weather.current.condition.text;
      }
      return '';
    }
  }
};
</script>

<style>
/* Reset default styles */
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

/* General styling for the app container */
#app {
  background-position: bottom;
  background-size: cover;
  transition: 0.4s;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
}

/* Styling for the default background when no weather location is available */
.default-background {
  background-image: url('./assets/default-bg.jpg');
}

/* Styling for warm weather background */
#app.warm {
  background-image: url('./assets/warm-bg.jpg');
  /* Add other warm weather styles here */
}

/* Styling for cold weather background */
#app.cold {
  background-image: url('./assets/cold-bg.jpg');
  /* Add your cold weather background styles here */
}

/* Styling for the main content area */
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

/* Styling for the search box */
.search-box {
  width: 100%;
  margin-bottom: 30px;
}

/* Styling for the search input bar */
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 5px 0px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px 0px 16px 0px;
  transition: 0.4s;
}

/* Styling for the focused search input bar */
.search-box .search-bar:focus {
  text-align: center;
  font-size: 32px;
  font-weight: 700;
  box-shadow: 0px 16px 0px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 0px 16px 0px 16px;
}

/* Styling for the location box */
.location-box .location {
  color: floralwhite;
  font-size: 32px;
  font-weight: 500;
  font-style: oblique;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

/* Styling for default view message */
h2 {
  color: floralwhite;
  padding: 20px;
  font-size: 32px;
  font-weight: 500;
  box-shadow: 0px 5px 0px rgba(0, 0, 0, 0.25);
  background-color: rgba(190, 180, 190, 0.5);
  border-radius: 5px;
}

/* Styling for default view section */
.default {
  padding-bottom: 20px;
}

/* Styling for the date in the location box */
.location-box .date {
  color: floralwhite;
  font-size: 20px;
  font-style: italic;
  font-weight: 300;
}

/* Styling for the temperature display */
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: floralwhite;
  font-size: 102px;
  font-style: oblique;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 60px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

/* Styling for the weather condition text */
.weather-box .weather {
  color: floralwhite;
  font-size: 48px;
  font-style: italic;
  font-weight: 700;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.75);
}

/* Styling for images */
img {
  border-radius: 30px;
  padding: 10px;
  margin: 20px auto;
  display: block;
}

</style>
