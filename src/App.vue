<template>
  <body :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <div id="app">
      <main>
        <div class="search-box">
          <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather">
        </div>
        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
          <div class="location">
            <p>{{ weather.name }}, {{ weather.sys.country }} </p>
          </div>
          <div class="date">
            <p>{{ formatDate() }}</p>
          </div>
          <div class="temp">{{ formatTemp(weather.main.temp) }}&deg;C</div>
          <div class="weather">
              <p>{{ weather.weather[0].main }}</p>
          </div>
        </div>
      </main>
    </div>    
  </body>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      api_key: 'c6b99773ddb351aa096bda3eeae85574',
      url_base: 'http://api.openweathermap.org/data/2.5/',
      query: '',
      date: '',
      isActive: false,
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if(e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResult);
      }
    },
    setResult(results) {
      this.weather = results;
    },
    formatTemp(temp) {
      var result = Math.floor(temp);
      return result;
    },
    formatDate() {
      let d = new Date();
      let days = ['Sunday','Monday','Tuesday','Wednesday','Thusday','Friday','Saturday'];
      let months = ['January','February','March','April','May','June','July','August','September','October','November','December'];
      //
      let date = d.getDate();
      let day = days[d.getDay()];
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`; 
    },
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Poppins:200,300,400,500,600,700,800,900&display=swap');
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}
body {
  width: 100%;
  height: 100vh;
  background: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: top;
  transition: .5s;
}
body.warm {
  background: url('./assets/warm-bg.jpg');
  background-size: cover;
  background-position: top;
}
main {
  width: 100%;
  padding: 25px;
  text-align: center;
}
#app {
  width: 100%;
  height: 100%;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.55));
}
.search-box {
  width: 50%;
  margin: 0px auto;
}
.search-box .search-bar {
  width: 100%;
  padding:10px;
  border: none;
  outline: none;
  border-radius: 15px;
  font-size: 18px;
  background-color: rgba(255,255,255,0.7);
  margin-bottom : 30px;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}
.weather-wrap {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.location {
  font-size: 35px;
  color: #fff;
}
.date {
  font-size: 22px;
  font-style: italic;
  color: gray;
  margin-bottom : 30px;
}
.temp {
  font-size: 70px;
  font-weight: bold;
  margin: 0 auto;
  padding:0px 25px;
  background:rgba(255,255,255,0.2);
  color: #fff;
  border-radius: 10px;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}
.weather {
  margin-top:20px;
  color: #fff;
  font-style: italic;
  font-size:40px;
  font-weight: bold;
}
</style>
