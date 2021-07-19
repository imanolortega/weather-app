<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 25
        ? 'appWarm'
        : 'appCold'
    "
  >
    <main>
      <div
        :class="
          typeof weather.main != 'undefined' && weather.main.temp > 25
            ? 'warm'
            : 'cold'
        "
      >
        <input
          type="text"
          name=""
          id=""
          class="search-bar"
          placeholder="Buscar..."
          v-model="query"
          @keypress="fetchWeather"
        />
        <div
          :class="
            typeof weather.main != 'undefined' && weather.main.temp > 25
              ? 'weather-wrap-warm'
              : 'weather-wrap-cold'
          "
          v-if="typeof weather.main != 'undefined'"
        >
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
    fetchWeatherStorage() {
      if (localStorage.query != "") {
        fetch(
          `${this.base_URL}weather?q=${localStorage.query}&units=metric&APPID=${this.apikey}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
      localStorage.query = this.query;
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
        "Domingo",
        "Lunes",
        "Martes",
        "Miércoles",
        "Jueves",
        "Viernes",
        "Sábado",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let hours = d.getHours();
      let minutes = d.getMinutes();

      return `${hours}:${minutes}, ${day} ${date} de ${month}`;
    },
  },
  mounted() {
    if (localStorage.query) {
      this.query = localStorage.query;
    }
    this.fetchWeatherStorage();
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
  font-family: "Quicksand", sans-serif;
}

#app {
  font-family: inherit;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  transition: 0.4s;
}

.appWarm {
  background-image: linear-gradient(135deg, #f97794 10%, #623aa2 100%);
}

.appCold {
  background-image: linear-gradient(135deg, #97abff 10%, #123597 100%);
}

main {
  min-height: 100vh;
  padding: 45px;
  display: flex;
  justify-content: center;
}

.cold {
  padding: 35px;
  width: 500px;
  margin-bottom: 30px;
  border-radius: 30px;
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
}

.warm {
  padding: 35px;
  width: 500px;
  margin-bottom: 30px;
  border-radius: 30px;
  background-image: url("./assets/warm-bg.jpg");
  background-size: cover;
  background-position: bottom;
}

.search-bar {
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

.search-bar:focus {
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

.weather-wrap-warm {
  margin: 20px 0px;
  border-radius: 20px;
  padding: 20px;
  background-image: linear-gradient(135deg, #f97794 10%, #623aa2 100%);
}

.weather-wrap-cold {
  margin: 20px 0px;
  border-radius: 20px;
  padding: 20px;
  background-image: linear-gradient(135deg, #97abff 10%, #123597 100%);
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

@media only screen and (min-device-width: 320px) and (max-device-width: 480px) and (-webkit-min-device-pixel-ratio: 2) {
  main {
    padding: 0px;
  }

  .cold {
    border-radius: 0px;
    min-height: 100vh;
    margin-bottom: 0px;
  }

  .warm {
    border-radius: 0px;
    min-height: 100vh;
    margin-bottom: 0px;
  }
}
</style>
