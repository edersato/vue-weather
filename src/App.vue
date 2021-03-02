<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 25 ? 'hot' : ''">
    <main>
      <div class="title">
        <h2>Clima App</h2>
      </div>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Busque uma cidade"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather-desc">{{ weather.weather[0].description }}</div>
        </div>
      </div>

      <div class="footer">
        <h5>Desenvolvido por <a href="https://edersato.github.io/portfolio/">Eder Rosa Sato</a></h5>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: '17cbaaac382b9f057f5d10130dd982b8',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      lang: 'pt_br',
      query: '',
      weather: {}
    };
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&lang=${this.lang}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho", "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"];
      let days = ["Domingo", "Segunda", "Terça", "Quarta", "Quinta", "Sexta", "Sábado"];
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
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
}

.title {
  color: #FFF;
  text-align: center;
  font-family: 'Turret Road', cursive;
  margin-bottom: 30px;
  text-shadow: 2px 3px rgba(0, 0, 0, 0.50);
}

#app {
  background-color: #85c1e9;
  background-size: cover;
  background-position: bottom;
  transition: 0.6s;
}

#app.hot {
  background-color: orange;
}

main {
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.50));
  min-height: 100vh;
  padding: 30px;
}

.search-box {
  margin-bottom: 50px;
  width: 100%;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #242122;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.40);
  border-radius: 0 16px 0 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0 16px 0;
}

.location-box .location {
  color: #fff;
  font-size: 30px;
  font-weight: 500;
  text-align: center;
  text-shadow: 2px 3px rgba(0, 0, 0, 0.50);
  margin-bottom: 5px;
}

.location-box .date {
  color: #fff;
  font-size: 17px;
  font-weight: 300;
  font-style: bold;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 5px 20px;
  color: #fff;
  font-size: 80px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather-desc {
  color: #fff;
  font-size: 36px;
  text-transform: capitalize;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  margin: 0.6em 0 1em 0;
}

.footer {
  color: #242122;
  text-align: center;
}

.footer a {
  color: white;
  text-decoration: none;
}

.footer a:hover {
  color: #242122;
  transition: 2s ease;
}
</style>
