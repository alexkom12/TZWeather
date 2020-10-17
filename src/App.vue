<template>
  <div id="app">
    <div class="bg_cities" :style="{ backgroundImage: `url( ${ bgOtherCity } )` }">
<!--      <img :src="require(`./assets/other-cities.png`)" alt="">-->
      <div class="bg_conditions">
        <div class="bg_blackout"></div>
      </div>
    </div>
    <div class="main">
      <div class="main_header">
        <h1>THE POGODA</h1>
      </div>
      <div class="main_box">
        <div class="box_search">
          <input
            type="text"
            class="main_search"
            placeholder="поиск..."
            v-model="mySearch"
            @keyup.enter="setCity(mySearch)"
            autofocus
          >
        </div>
      </div>
      <div class="main_favorite">
        <button class="favorite_cities" v-for="(city, index) in cities"
                @click="setCity(city.name)" :key="index">{{ city.name }}</button>
      </div>
      <div class="main_indications" v-if="myWeather">
        <div class="indications_location">{{myWeather.name}}</div>
        <div class="indications_main">
          <div class="indications_wind">
            <div class="wind_speed">{{myWeather.wind.speed}} м/с</div>
<!--            <div class="wind_deg">{{myWeather.wind.deg}}</div>-->
          </div>
          <div class="indications_temp">{{Math.round(myWeather.main.temp)}}°C</div>
          <div class="indications_clouds">{{myWeather.weather[0].main}}</div>
        </div>
        <div class="indications_icons">
          <img :src="require(`./assets/icons/${myWeather.weather[0].icon}@2x.png`)" alt="#">
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  data: () => ({
    api_key: 'e0bc802ff8fa4f2c8549686f67a50357',
    url_base: 'https://api.openweathermap.org/data/2.5/',
    mySearch: '',
    myWeather: null,
    myStatus: '',
    cities: [
      { name: 'Москва', url: '@/assets/C-Moscow.png' },
      { name: 'Париж', url: 'assets/C-Paris.png' },
      { name: 'Нью-Йорк', url: 'assets/С-New-York.png' },
    ],
    bgOtherCity: 'assets/other-cities.png',
  }),
  mounted() {
    if (!navigator.geolocation) {
      this.status = 'Разрешите доступ к геопозиции';
    } else {
      this.status = 'Определяю...';
      navigator.geolocation.getCurrentPosition((position) => {
        this.status = '';
        this.getCurrentWeather(position.coords.latitude, position.coords.longitude);
      }, () => {
        this.status = 'Разрешите доступ к геопозиции';
      });
    }
  },
  methods: {
    setCity(city) {
      let result = {};
      result = this.cities.find((item) => item.name.toLowerCase() === city.toLowerCase());
      // eslint-disable-next-line no-mixed-operators
      this.bgOtherCity = result && result.url || 'assets/other-cities.png';
      this.$axios.get(`${this.url_base}weather?q=${city}&units=metric&lang=ru&appid=${this.api_key}`)
        .then((res) => {
          this.myWeather = res.data;
        });
    },
    getCurrentWeather(lat, long) {
      this.$axios
        .get(`https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${long}&units=metric&lang=ru&appid=d0e89fc89523134758ba52e2afdb6674`)
        .then((res) => {
          this.myWeather = res.data;
        });
    },
  },
};
</script>

<style lang="css">
  /*Обнуление*/
  *{
    padding: 0;
    margin: 0;
    border: 0;
  }
  *,*:before,*:after{
    -moz-box-sizing: border-box;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
  }
  :focus,:active{outline: none;}
  a:focus,a:active{outline: none;}

  nav,footer,header,aside{display: block;}

  html,body, button{
    height: 100%;
    width: 100%;
    font-size: 100%;
    line-height: 1;
    font-size: 18px;
    line-height: 21px;
    color: #333;
    -ms-text-size-adjust: 100%;
    -moz-text-size-adjust: 100%;
    -webkit-text-size-adjust: 100%;
  }
  input,button,textarea{font-family:inherit;}

  input::-ms-clear{display: none;}
  button{cursor: pointer;}
  button::-moz-focus-inner {padding:0;border:0;}
  a, a:visited{text-decoration: none;}
  a:hover{text-decoration: none;}
  ul li{list-style: none;}
  img{vertical-align: top;}

  h1,h2,h3,h4,h5,h6{font-size:inherit;font-weight: 400;}
  /*--------------------*/
  /* подключение шрифтов*/
  @font-face {
  font-family: 'RobotoCondensed-Bold';
  src: url('assets/fonts/RobotoCondensed-Bold.ttf');
  font-weight: normal;
  font-style: normal;
  }

  @font-face {
  font-family: 'RobotoCondensed-Regular';
  src: url('assets/fonts/RobotoCondensed-Regular.ttf');
  font-weight: normal;
  font-style: normal;
  }
html{scroll-behavior: smooth;}
body{
  font-family: RobotoCondensed-Regular;
  width: 100vw;
  color: #333333;
}
  #app {
    display: block;
    width: 100vw;
    height: 100vh;
  }

    .bg_cities {
      display: block;
      position: absolute;
      width: 100%;
      height: 100%;
      background-image: url("assets/img-start-m.png");
      background-size: 60%;
      background-position: 50% 100%;
      background-repeat: no-repeat;
    }
    .bg_cities img {
      width: 100%;
    }
    .bg_conditions {
      display: block;
      position: absolute;
      z-index: -1;
      width: 100%;
      height: 100%;
      background-color: #75bbfd;
    }

    .main {
      display: block;
      position: relative;
      z-index: 10;
      padding: 20px;
      max-width: 500px;
      margin: 0 auto;
    }

    .main_header {
      display: flex;
      justify-content: center;
      height: 100%;
    }

    .main_header h1 {
      font-size: 2em;
      line-height: 2em;
      cursor: default;
    }
    .main_box{
      width: 100%;
    }

    .main_box .box_search {
      display: flex;

      width: 100%;
    }

    .main_box .box_search input {
      display: block;
      font-size: 24px;
      margin: 5px 0px 10px 0px;
      background-color: rgba(255, 255, 255, 0.7);
      width: 100%;
      height: 2em;
      padding: 0 20px 0 20px;
      border-radius: 4px;
    }
    .main_favorite{
      display: flex;
      justify-content: space-between;
    }

    .favorite_cities{
      display: flex;
      justify-content: center;
      font-size: 18px;
      padding: 5px;
      align-items: center;
      width: 100%;

      border-radius: 4px;
      background-color: rgba(255,255,255, 0.5);
    }
    .favorite_cities:first-child{
      margin-right: 10px;
    }
    .favorite_cities:last-child{
      margin-left: 10px;
    }
    .main_indications{
      display: block;
      margin: auto;
      text-align: center;
      font-size: 28px;
    }

    .indications_location{
      height: 30px;
      margin-top: 20px;
      cursor: default;
    }

    .indications_main{
      display: flex;
      justify-content: center;
      height: auto;
      width: 100%;
      margin-bottom: 15px;
      margin-top: 10px;
      cursor: default;
    }

    .indications_temp{
      font-weight: bold;
      font-size: 42px;
      line-height: 40px;
    }

    .indications_wind, .indications_temp, .indications_clouds{
      width: 100%;
    }
    .indications_wind{
      margin: auto;
    }
    .indications_clouds{
      margin: auto;
      margin-left: 0;
      margin-right: 0;
    }

    .indications_icons{
      z-index: 12;
      align-items: center;
    }

    .indications_icons img{
      width: 100px;
      height: 100px;
    }
</style>
