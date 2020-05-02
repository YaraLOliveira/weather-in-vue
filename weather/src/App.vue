<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 18 ? 'warm' : 'app' " >
    <main>
      <div class="search-box">
        <input class="search-bar" type="text" placeholder="localização.." v-model="state" @keypress="fetchClim"/>
      </div>
      <div class="content" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name}}, {{weather.sys.country}} </div>
          <div class="date">{{timeBuilder()}} </div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°c </div> <!--  -->
          <img class="icon" v-bind:src="`http://openweathermap.org/img/wn/${weather.weather[0].icon}@2x.png`"  />
          <div class="weather" >{{weather.weather[0].description}} </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      api_key: "2b8cf3d65d7953a964a6f1c8b90f60ba",
      api_url: "https://api.openweathermap.org/data/2.5/",
      country:"",
      state: "",
      icon: "",
      weather: ""
    };
  },
  methods: {
    fetchClim (e){
      if(e.key == "Enter"){
        fetch(`${this.api_url}weather?q=${this.state}&units=metric&appid=${this.api_key}&lang=pt_br`)//documentação
        .then(res =>{
          return res.json();
        })
        .then(this.setResult);
      }
    },

    setResult (resul) {
      this.weather = resul;
    },
    timeBuilder () {
      let datas = new Date();
      let months = ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho", "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"];
      let day = ["Domingo", "Segunda-feira", "Terça-feira", "Quarta-feira", "Quinta-feira", "Sexta-feira", "Sabado"];

      const dia = day[datas.getDay()];
      const data = datas.getDate();
      const mes = months[datas.getMonth()];
      const ano = datas.getFullYear();

      return `${dia} ${data} ${mes} ${ano}`
    }
  }
};
</script>

<style>
* {
  margin: 0; padding: 0;
  box-sizing: border-box; /* sem borda */
}
body {
  font-family: OCR A Std, monospace; /* fonte */
  text-align: center;
}
#app {
  background-image: url('./assets/snow.jpg');
  background-size: cover; /* full background */
  background-position: bottom; /* inferior */
  transition: 0.5s;
}

#app.warm {
  background-image: url('./assets/sun.jpg');
}
main {
  min-height: 100vh; /* a definir a altura mínima de um determinado elemento. */
  padding: 20px;
  background-image:linear-gradient(to bottom ,rgba(0,0,0,0.25 ), rgba(0,0,0,0.75 ));
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;


  color: #000000;
  font-size: 20px;

  appearance: none;
  bottom: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0,0,0,0.25 );
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25 );
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: #fff;
  margin: 5%;
  font-size: 28px;
  font-weight:500;/* intensidade da fonte */
  text-align: center;
  text-shadow: 3px 3px rgba(0,0,0, 0.25 );
}
.location-box .date {
  color: #fff;
  margin: 2%;
  font-size: 20px;
  font-weight: 500;
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
  font-size: 60px;
  font-weight: 900;


  text-shadow: 3px 6px rgba(0, 0, 0, 0.25 );
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px 0px 16px 0px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0,25 );
}
.weather-box .icon {
  display: inline-block;
  color: #ffffff;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25 );
  /* background-color: rgba(0, 0, 0, 0.25); */
  border-radius: 16px 0px 16px 0px;
  margin: 10px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0,25 );
}
.weather-box .weather {
  margin: 5%;
  color: #fff;
  font-size: 40px;
  font-weight: 600;
  font-style: italic;
  text-transform: capitalize;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}


</style>
