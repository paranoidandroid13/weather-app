<template>
  <div>
    <b-container class="h-100 ">
      <b-row class="h-100 justify-content-center align-items-center">
        <b-col class="col-md-6 p-0">
          <div class="row no-gutters overflow-hidden flex-md-row mb-4 position-relative">
            <div class="col-md-12 p-4 d-flex flex-column align-items-center position-static">
              <strong class="h3 d-inline-block mb-2 text-primary">{{ currentTemp.mediumTemp }}°C</strong>
              <h6>{{ currentTemp.description}}</h6>
              <img v-bind:src="currentTemp.actual_icon" height="100" width="100" alt="">
              <h4 class="mb-4 city">Rostov-on-Don</h4 >
              <h6>{{ convertToDay(currentTemp.date) }}</h6>
            </div>

            <div class="w-100"></div>

            <div class="col-md-12 d-flex align-items-center p-4 flex-md-row">
              <div class="col-md-4">{{ convertToDay(nextDay1.date) }}</div>
              <div class="col-md-4 d-flex flex-start">
                <img v-bind:src="nextDay1.actual_icon" height="50" width="50" alt="">
                <h6 id="descr" class="ml-2 mt-3">{{ nextDay1.description }}</h6>
              </div>
              <div class="col-md-4 text-primary">{{ nextDay1.mediumTemp }}°C</div>

            </div>

            <div class="w-100"></div>

            <div class="col-md-12 d-flex align-items-center p-4">
              <div class="col-md-4">{{ convertToDay(nextDay2.date) }}</div>
              <div class="col-md-4 d-flex flex-start">
                <img v-bind:src="nextDay2.actual_icon" height="50" width="50" alt="">
                <h6 id="descr" class="ml-2 mt-3">{{ nextDay2.description }}</h6>
              </div>
              <div class="col-md-4 text-primary">{{ nextDay2.mediumTemp }}°C</div>
            </div>

            <div class="w-100"></div>

            <div class="col-md-12 d-flex align-items-center p-4">
              <div class="col-md-4">{{ convertToDay(nextDay3.date) }}</div>
              <div class="col-md-4 d-flex flex-start">
                <img v-bind:src="nextDay3.actual_icon" height="50" width="50" alt="">
                <h6 id="descr" class="ml-2 mt-3">{{ nextDay3.description }}</h6>
              </div>
              <div class="col-md-4 text-primary">{{ nextDay3.mediumTemp }}°C</div>
            </div>
          </div>
        </b-col>
      </b-row>
    </b-container>
  </div>

</template>

<script>
  export default {
    data() {
      return {
        url_base: 'https://api.openweathermap.org/data/2.5/onecall?lat=47.24&lon=39.71&exclude=current,minutely,hourly,alerts&units=metric&appid=',
        api_key: '891023a344fa8b274da8b0007a2e343d',
        currentTemp: {
          mediumTemp: '',
          description: '',
          icon: '',
          actual_icon: '',
          date: '',
        },
        nextDay1: {
          mediumTemp: '',
          description: '',
          icon: '',
          actual_icon: '',
          date: '',
        },
        nextDay2: {
          mediumTemp: '',
          description: '',
          icon: '',
          actual_icon: '',
          date: '',
        },
        nextDay3: {
          mediumTemp: '',
          description: '',
          icon: '',
          actual_icon: '',
          date: '',
        }
      }
    },
    mounted() {
      this.fetchData()
    },
    methods: {
      fetchData() {
      fetch(`${this.url_base}${this.api_key}`)
      .then((res) => res.json()).then(json => {
        this.currentTemp.mediumTemp = Math.round((json.daily[0].temp.min + json.daily[0].temp.max) / 2)
        this.currentTemp.description = json.daily[0].weather[0].description
        this.currentTemp.icon = json.daily[0].weather[0].icon
        this.currentTemp.actual_icon = `http://openweathermap.org/img/wn/${this.currentTemp.icon}@2x.png`
        this.currentTemp.date = json.daily[0].dt

        this.nextDay1.mediumTemp = Math.round(json.daily[1].temp.day)
        this.nextDay1.description = json.daily[1].weather[0].description
        this.nextDay1.date = json.daily[1].dt
        this.nextDay1.icon = json.daily[1].weather[0].icon
        this.nextDay1.actual_icon = `http://openweathermap.org/img/wn/${this.nextDay1.icon}@2x.png`

        this.nextDay2.mediumTemp = Math.round(json.daily[2].temp.day)
        this.nextDay2.description = json.daily[2].weather[0].description
        this.nextDay2.date = json.daily[2].dt
        this.nextDay2.icon = json.daily[2].weather[0].icon
        this.nextDay2.actual_icon = `http://openweathermap.org/img/wn/${this.nextDay2.icon}@2x.png`

        this.nextDay3.mediumTemp = Math.round(json.daily[3].temp.day)
        this.nextDay3.description = json.daily[3].weather[0].description
        this.nextDay3.date = json.daily[3].dt
        this.nextDay3.icon = json.daily[3].weather[0].icon
        this.nextDay3.actual_icon = `http://openweathermap.org/img/wn/${this.nextDay3.icon}@2x.png`
      })
      },
      convertToDay(time) {
        const days = ['SUN', 'MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT']
        const numDay = new Date(time * 1000).getDay()
        return days[numDay]
      }
    }
  }
</script>

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
