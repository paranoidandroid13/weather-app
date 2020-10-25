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
            <h4 class="mb-0 city">Rostov-on-Don</h4 >
            <h3>{{ convertToDay(daily.date) }}</h3>
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
            <div class="col-md-4">desct</div>
            <div class="col-md-4">temp</div>
          </div>
          <div class="w-100"></div>
          <div class="col-md-12 border solid red d-flex align-items-center p-4">
             <div class="col-md-4">date</div>
            <div class="col-md-4">desct</div>
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

// var days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
// var d = new Date(data.list[0].dt * 1000);
// var dayName = days[d.getDay()];
// console.log(dayName)

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
        },
        daily: {
          date: ''
        },
      }
    },
    mounted() {
      console.log('weather! fuckkkkk')
      this.fetchData()
      this.fetchData4Days()
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
        this.daiy.date = json.list[0].dt
      })
      },
      convertToDay(time) {
        const days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
        const newDate = new Date(time * 1000)
        return days[newDate.getDay()]
      },
      fetchData4Days() {
        fetch('https://api.openweathermap.org/data/2.5/forecast?q=Rostov-on-Don,ru&units=metric&appid=891023a344fa8b274da8b0007a2e343d')
        .then((res) => res.json())
        .then(json => {
          let arr = json.list
          // for (let i=0; i<28; i++) {
          //   const arrTemps = arr[i].main.temp
          //   console.log(arrTemps)
          // }

          // const arrTemps = arr.map((item) => [ ...item.main.temp ])
          // console.log(arrTemps)
          const tempsArr = arr.map((item) => Object.values(item)[1].temp)
          // tempsFiltered = tempsArr.filter(item, index) => index >= 4 && index <= 28 ))
          console.log(tempsArr)
          const filteredTemps = tempsArr.slice(4, 27)
          console.log(filteredTemps)
          // const we25 = filteredTemps.slice(0, 7)
          // console.log(we25)
          // // const we26 = filteredTemps.slice(7, 15)
          // // const we27 = filteredTemps.slice(15, 23)
          // const medium25 = we25.reduce((acc, cur) => acc + cur, 0)
          // const medium25_res = medium25 / we25.length
          // console.log(medium25_res)
          // СРЕДНЯЯ ПОГОДА ЗА ТЕКУЩИЙ ДЕНЬ
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
