<template>
  <div>
  <b-container>
    <b-row>
      <b-col class="col-md-6 border p-0">
        <div class="row no-gutters border rounded overflow-hidden flex-md-row mb-4 position-relative">
          <div class="col-md-12 border solid red p-4 d-flex flex-column align-items-center position-static">
            <strong class="h3 d-inline-block mb-2 text-primary">{{ currentTemp.temp }}°C</strong>
            <h6>{{ currentTemp.description}}</h6>
            <img v-bind:src="currentTemp.actual_icon" height="100" width="100" alt="">
            <h4 class="mb-4 city">Rostov-on-Don</h4 >
            <h6>{{ convertToDay(currentTemp.date) }}</h6>
          </div>
          <div class="w-100"></div>
          <div class="col-md-12 border solid red d-flex align-items-center p-4 flex-md-row">
            <div class="col-md-4">date</div>
            <div class="col-md-4">
              <img v-bind:src="currentTemp.actual_icon" alt="">
            </div>
            <div class="col-md-4">temp</div>

            </div>
          <div class="w-100"></div>
          <div class="col-md-12 border solid red d-flex align-items-center p-4">
            <div class="col-md-4">date</div>
            <div class="col-md-4">desc</div>
            <div class="col-md-4">temp</div>
          </div>
          <div class="w-100"></div>
          <div class="col-md-12 border solid red d-flex align-items-center p-4">
             <div class="col-md-4">date</div>
            <div class="col-md-4">desc</div>
            <div class="col-md-4">temp</div>
          </div>

        </div>
      </b-col>
    </b-row>
  </b-container>
  </div>

</template>

<!-- <script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>
-->

<script>
  export default {
    data() {
      return {
        url_base: 'https://api.openweathermap.org/data/2.5/weather?q=Rostov-on-Don&units=metric&appid=',
        api_key: '891023a344fa8b274da8b0007a2e343d',
        currentTemp: {
          temp: '',
          feels: '',
          description: '',
          icon: '',
          actual_icon: '',
          date: '',
        }
      }
    },
    mounted() {
      this.fetchData()
      // this.fetchData4Days()
    },
    methods: {
      fetchData() {
      fetch(`${this.url_base}${this.api_key}`)
      .then((res) => res.json()).then(json => {
        this.currentTemp.temp = Math.round(json.main.temp)
        this.currentTemp.feels = Math.round(json.main.feels_like)
        this.currentTemp.description = json.weather[0].description
        this.currentTemp.icon = json.weather[0].icon
        this.currentTemp.actual_icon = `http://openweathermap.org/img/wn/${this.currentTemp.icon}@2x.png`
        this.currentTemp.date = json.dt
        console.log(this.currentTemp.date)
      })
      },
      convertToDay(time) {
        const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
        const numDay = new Date(time * 1000).getDay()
        return days[numDay]
      },
      fetchData4Days() {
        fetch('https://api.openweathermap.org/data/2.5/forecast?q=Rostov-on-Don,ru&units=metric&appid=891023a344fa8b274da8b0007a2e343d')
        .then((res) => res.json())
        .then(json => {
          let arr = json.list
          const tempsArr = arr.map((item) => Object.values(item)[1].temp)
          console.log(tempsArr)
        })
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
