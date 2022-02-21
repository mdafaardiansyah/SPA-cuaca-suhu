<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ?'warm' : ''">
    <main>
      <div class="search-box">
        <input 
        type="text" 
        class="search-bar" 
        placeholder="Cari..."
        v-model="query"
        @keypress="fetchWeather"
        />
      </div>
    
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location"> {{ weather.name }}, {{ weather.sys.country }} </div>
          <div class="date">{{ dateBuilder() }} </div>
        </div>
        

        <div class="weather-box">
          <div class="temp">{{ Math.ceil(weather.main.temp) }}°C</div>
          <div class="weather"> {{ weather.weather[0].main }} </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key:'8a592093f702bbd2d7ee25dc1609c69b',
      url_base:'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    async fetchWeather(e) {
      if (e.key == "Enter") {
        let response = await fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`);
        let json = await response.json();
        this.weather = await json;

      }
    },
    setResults (results){
      this.weather = results;
    },
    dateBuilder (){
      let d = new Date();
      let months =["January", "February", "March", "April", "May", 
      "June", "July", "August", "September", "October", "November", "December"];

      let days =["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

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
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'Roboto', sans-serif;
}

#app{
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition:0.4s;
}

#app.warm{
  background-image: url('./assets/warm-bg.jpg');
}

main{
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}

.search-box{
    width: 100%;
    margin-bottom: 30px;
  }

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color:#313131;
  font-size: 20px;

  appearance: none;
  border : none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0,0,0,0.5);
  background-color: rgba(255,255,255,0.5);
  border-radius: 20px 20px 20px 20px;
  transition: 0.8s;
}

.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0,0,0,0.5);
  background-color: rgba(255,255,255,0.75);
  border-radius: 20px 0px 20px 0px;
}

.location-box .location{
  color: #FFF;
  font-size: 32px;
  font-weight: bold;
  text-align: center;
  text-shadow: 2px 2px rgba(0,0,0,0.5);
}

.location-box .date{
  color: #FFF;
  font-size: 20px;
  font-weight: 100;
  font-style: italic;
  text-align: center;
}

.weather-box{
  text-align: center; 
}

.weather-box .temp{
  display: inline-block;
  padding: 25px 25px;
  color: #FFF;

  font-size: 100px;
  font-weight: 900;

  text-shadow: 4px 7px rgba(0,0,0,0.5);
  background-color: rgba(255,255,255,0.25);
  border-radius: 30px 30px 30px 30px;
  margin: 30px 0px;

  box-shadow: 4px 8px rgba(0,0,0,0.2);
}

.weather-box .weather{
  color: #FFF;
  font-size: 64px;
  font-weight: bold;
  font-style: italic;
  text-shadow: 4px 7px rgba(0,0,0,0.5);
}

</style>
