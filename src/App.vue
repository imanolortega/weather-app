<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 20 ? 'warm' : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          name=""
          id=""
          class="search-bar"
          placeholder="Buscar..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temperature">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      apikey: "468dc9f36aad1cf285ec86d73471f77c",
      base_URL: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.base_URL}weather?q=${this.query}&units=metric&APPID=${this.apikey}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
      console.log(this.weather);
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "Enero",
        "Febrero",
        "Marzo",
        "Abril",
        "Mayo",
        "Junio",
        "Julio",
        "Agosto",
        "Septiembre",
        "Octubre",
        "Noviembre",
        "Diciembre",
      ];
      let days = [
        "Lunes",
        "Martes",
        "Miércoles",
        "Jueves",
        "Viernes",
        "Sábado",
        "Domingo",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let hours = d.getHours();
      let minutes = d.getMinutes();

      return `${hours}:${minutes}, ${day} ${date} de ${month}`;
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
  font-family: "Inter", sans-serif;
}

#app {
  font-family: inherit;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #2c3e50;
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.15),
    rgba(0, 0, 0, 0.55)
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
  font-size: 1rem;

  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.35);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
}

.location-box .location {
  color: #fafafa;
  font-size: 1.6rem;
  text-align: center;
  font-weight: 500;
}

.location-box .date {
  color: #fafafa;
  font-size: 1.2rem;
  text-align: center;
  font-weight: 500;
  font-style: italic;
  margin: 2px 0px;
}

.weather-box {
  text-align: center;
}

.weather-box .temperature {
  display: inline-block;
  color: #fafafa;
  padding: 10px 25px;
  font-size: 3.5rem;
  font-weight: 500;
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 0px 16px 0px 16px;
  padding: 10px 25px;
  margin: 30px 0px;
}

.weather-box .weather {
  color: #fafafa;
  font-size: 1.6rem;
  text-align: center;
  font-weight: 600;
  letter-spacing: 0.1rem;
}
</style>
