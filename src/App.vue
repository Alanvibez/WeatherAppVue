<template>
<div class="bg" @click="isOpen = false">
  <div class="container">
    <h1 class="animate__animated animate__fadeInTopLeft">Weather</h1>
    <input @keyup.enter="inputCheck" v-model="inputCity" class="animate__animated animate__fadeInBottomRight" placeholder="Search..." type="text">
    <h3 class="error" v-if="notFound">City not found</h3>
    <div class="weather" :class="{open:isOpen}">
      <div class="city">{{weather.city}}, {{weather.country}}</div>
      <div class="date">{{dateBuilder()}}</div>
      <h1 class="deg">{{weather.deg}}°C</h1>
      <div class="humidity">
          <div class="feel">{{weather.feel}}°C<span>feel</span></div>
          <div class="feel">{{weather.hum}}%<span>humidity</span> </div>
      </div>
      <h2 class="pog">{{weather.pog}}</h2>
    </div>
  </div>
</div>
</template>

<script>


export default {
  name: 'App',
   data(){
        return{
          api_key:'54210c6a4bb4e7365e79adf1006df189',
          url_base:`https://api.openweathermap.org/data/2.5/weather?`,
          inputCity:'',
          notFound: false,
          isOpen:false,
            weather:{
                city:'Moscow', 
                country:'RU', 
                deg:0,
                feel:0,
                date:'24 June',
                pog:'Clouds',
                hum:0
            }     
        }
    },
  components: {
   
  },
  methods:{
  
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },

    inputCheck(e){
      if(e.key == 'Enter'&&this.inputCity.length > 0&&!/\d/.test(this.inputCity)){
        this.fetchWeather()
      }
      else{
        this.notFound = true
      }
    },


    fetchWeather:async function(){
      const response = await fetch(`${this.url_base}q=${this.inputCity}&appid=${this.api_key}&units=metric`)
      if(response.ok){
        this.notFound = false
        const data = await response.json()
        this.weather.city = data.name
        this.weather.country = data.sys.country
        this.weather.deg = Math.round(data.main.temp)
        this.weather.pog = data.weather[0].description
        this.weather.feel = Math.round(data.main.feels_like)
        this.weather.hum = Math.round(data.main.humidity)
        this.isOpen = true
      }else{
        this.notFound = true
      }
      

    }
  }
}
</script>

<style>

</style>
