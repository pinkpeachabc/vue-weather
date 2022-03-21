<template>
  <div id="app" :class=" weather.temp < 16 ? 'cold' : ' ' ">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather" />
      </div>

      <div class="weather-wrap" v-if="typeof location.name != 'undefined'">
        <div class="location-box">
          <div class="location">{{location.name}},{{location.adm1}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{weather.temp}}°c</div>
          <div class="weather">{{weather.text}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      api_key: 'ff27c3a2d38e4de39d7d38b425c4f926',
      url_base: 'https://devapi.qweather.com/v7/weather/now?',
      url_location: 'https://geoapi.qweather.com/v2/city/lookup?',
      query: '',
      weather: {},
      location: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == 'Enter') {
        fetch(`${this.url_location}key=${this.api_key}&location=${this.query}`, { method: 'get' }).then((response) => {
          return response.json()
        }).then((result) => {
          this.location = eval(result).location[0]
          console.log(result)
          fetch(`${this.url_base}key=${this.api_key}&location=${this.location.id}`, { method: 'get' }).then((response) => {
            return response.json()
          }).then((result) => {
            this.weather = eval(result).now
            console.log(result)
          })
        })
      }
    },
    dateBuilder() {
      let d = new Date();
      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']

      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()

      return `${day} ${date} ${month} ${year}`
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "montserrat", sans-serif;
}

#app {
  background-image: url("./assets/warm.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.cold {
  background-image: url("./assets/cold.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
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

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
