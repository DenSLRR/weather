<template>
  <div class="wrapper">
    <h1>Weather app </h1>
    <p>Find out the weather in your : {{city == '' ? 'Lead your city' : cityName}}</p>
    <input v-model="city" type="text" placeholder="City">
    <button
    @click="getWeather()"
     v-if="city != ''"
     >Get weather</button>
    <button disabled v-else>Enter the name of the city</button>
    <p class="error">{{error}}</p>
    
      <div v-if="info != null">
          <p>{{cityName}}</p>
          <p>{{cityTemp}}</p>
          <p>{{cityFeelsLike}}</p>
          <p>{{cityMinTemp}}</p>
          <p>{{cityMaxTemp}}</p>
      </div>
  </div>
</template>

<script>
import axios from 'axios';


  export default {
    
    data() {
      return {
        city: '',
        error: '',
        info: null,
      }
    },
    computed: {
      cityName() {
        return "'" + this.city + "'"
      },
      cityTemp() {
        return 'Температура ' + this.info.main.temp
      },
      cityFeelsLike() {
        return 'Ощущается как ' + this.info.feels_like
      },
      cityMinTemp() {
        return 'Минимальная температура ' + this.info.main.temp_min
      },
      cityMaxTemp() {
        return 'Максимальная температура ' + this.info.main.temp_max
      },
    }, 
    methods: {
      getWeather() {
        if(this.city.trim().length < 2) {
          this.error = 'Enter a name of more than 1 character :)';
          return false;
        }
         this.error = ''
           axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units&appid=d985184bf7ca8df20fe078b828387290`)
            .then(res => (this.info = res.data)) 


      }
    }

  }
</script>

<style  scoped>
  .wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    padding: 20px;
    background: #1f0f24;
    text-align: center;
    color: #fff ;
  }

  .wrapper h1 {
    margin-top: 50px;
  }

  .wrapper p {
    margin-top: 20px;
  }
  .wrapper input {
    margin-top: 30px;
    background-color: transparent;
    border: 0;
    border-bottom: 2px solid #110813;
    color: #fcfcfc;
    font-size: 14px;
    padding: 5px 8 px;
    outline: none;
  }

  .wrapper input:focus {
    border-bottom-color: #6e2d7d;
  }
  .wrapper button:disabled {

    background-color: #6e2d7d;
    cursor: not-allowed;

  }
  .wrapper button {
    background-color: #e3bc4b;
    color: #fff;
    border-radius: 10px;
    border: 1px solid #b99935;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
    
  }
  .wrapper button:hover {
    transform: scale(1.1) translateY(-5px);
  }

  .error {
    color: #d03939;
  }
</style>