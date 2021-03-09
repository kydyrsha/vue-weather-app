<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input
            type="text"
            class="search-bar"
            placeholder="search..."
            v-model="query"
            @keypress="fetchWeather"
        >
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      api_key: '96c57fe8db54c9584cb76a27a1c21b41',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res=> {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
* {
  margin:0;
  padding:0;
  box-sizing: border-box;
}

body {
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url('https://i.postimg.cc/hvNWKVfD/tristen-lee-nvlan-P72-Ew0-unsplash.jpg');
  background-size: cover;
  background-position: center;
  transition: 0.4s;
}

#app.warm {
  background: url('https://i.postimg.cc/nzLvTCzb/kyle-thacker-9ric71-H9ulk-unsplash.jpg');
  background-size: cover;
  background-position: center;
  transition: 0.4s;
}

main {
  min-height: 100vh;
  padding:2rem;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width:100%;
  margin-bottom: 2rem;
}

.search-box .search-bar {
  display: block;
  width:100%;
  padding:1rem;
  color:#313131;
  font-size: 1.5rem;
  appearance: none;
  border: none;
  outline: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0 0.25);
  background: rgba(255, 255, 255, 0.5) none;
  border-radius: 0 1rem 0 1rem;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 1rem rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255 0.75);
  border-radius: 1rem 0 1rem 0;
}

.location-box .location {
  color:#fff;
  font-size: 2rem;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0, 0.25);
}

.location-box .date {
  color:#fff;
  font-size: 1.25rem;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding:0.75rem 1.5rem;
  color:#fff;
  font-size: 5.5rem;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0, 0.25);
  background-color: rgba(255,255,255, 0.25);
  border-radius: 1rem;
  margin:1.5rem 0;
  box-shadow: 3px 6px rgba(0,0,0, 0.25);
}

.weather-box .weather {
  color:#fff;
  font-size: 3rem;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0, 0.25);
}
</style>
