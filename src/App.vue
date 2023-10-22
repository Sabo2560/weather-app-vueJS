<template>
  <div id="app" :class="[weather.location && weather.current.temp_c > 16 ? 'warm' : '']">


    <main>
      <div class="search-box">
        <input type="text"
               class="search-bar" 
               placeholder="Search..." 
               v-model="query"
               @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="weather.location">
        <div class="location-box">
          <div class="location">{{ weather.location.name }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ weather.current.temp_c }}°C</div>
          <div class="weather">{{ weather.current.condition.text }}</div>
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
    fetchWeather(e) {
      if (e.key === 'Enter') {
        fetch(`${this.url_base}current.json?q=${this.query}&key=${this.api_key}&aqi=no`)
          .then((res) => {
            return res.json();
          })
          .then((data) => {
            this.setResults(data);
          });
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
};
</script>

<style>

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
#app {
  background-image: url('./assets/cold-bg.jpg');
  background-position: bottom;
  background-size: cover;
  transition: 0.4s;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  
}
#app.warm {
  background-image: url('./assets/warm-bg.jpg');
  /* Add other warm weather styles here */
}
main{
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom,rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}
.search-box{
  width: 100%;
  margin-bottom: 30px;
}

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
  background-color: rgba(255,255,255,0.5);
  border-radius: 16px 0px 16px 0px;
  transition: 0.4s;
}

.search-box .search-bar:focus{
  box-shadow: 0px 16px 0px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255,0.75);
  border-radius: 0px 16px 0px 16px;
}

.location-box .location{
  color: floralwhite;
  font-size: 32px;
  font-weight: 500;
  font-style: oblique;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date{
  color: floralwhite;
  font-size: 20px;
  font-style: italic;
  font-weight: 300;
}
.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: floralwhite;
  font-size: 102px;
  font-style: oblique;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 60px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0,0,0,0.25);
}
.weather-box .weather{
  color: floralwhite;
  font-size: 48px;
  font-style: italic;
  font-weight: 700;

  text-shadow: 3px 6px rgba(0,0,0,0.75);
}
</style>
