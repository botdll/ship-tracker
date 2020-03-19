<template>
  <b-container fluid>

    <b-row class="b-color mb-3">
      <b-col cols="12 mb-4 mt-4">        
        <h1 class="text-justify text-center text-light display-5">
          <b-img center src="../assets/marine_logo.png" alt="logo"></b-img>Ship Tracker
        </h1>
      </b-col>
    </b-row>

    <b-row>

      <b-col cols="12 mb-3" md="5">
        <ShipList
                :ships="ships"
                @submitted="updateData($event)"
                @mouse-over="over"
                @mouse-leave="leave"/>
      </b-col>      
      
      <b-col cols="12" md="7">
        <ShipMap :ships="ships"/>
      </b-col>

    </b-row>
    
  </b-container>
</template>

<script>
import axios from 'axios'
import ShipList from './ShipList'
import ShipMap from './ShipMap'

export default {
  name: 'Main',
  components: {
    ShipList,
    ShipMap
  },
  data() {
    return {
      ships: [],
      apiKey: process.env.VUE_APP_API_KEY,
      mmsi: '',
      days: '',
      normalIcon: [25, 25],
      largeIcon: [50, 50]
    }
  },
  methods: {
    updateData(userInput) {
      userInput.loading = true
      axios.get(`https://services.marinetraffic.com/api/exportvesseltrack/${this.apiKey}/v:2/period:daily/days:${userInput.days}/mmsi:${userInput.mmsi}/protocol:jsono`)
           .then(res => {
             userInput.loading = false
             this.ships = res.data.map(res => {
              res.iconSize = this.normalIcon
              return res
              })
           })
           .catch(err => {
             userInput.loading = false
             alert('There was an issue with data fetching')
             console.log(err)
           })
    },
    over(index) {
      this.ships[index].iconSize = this.largeIcon
    },
    leave(index) {
      this.ships[index].iconSize = this.normalIcon
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.b-color {
background-color: #00186B;
}
</style>
