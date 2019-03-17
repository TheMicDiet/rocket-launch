<template>

  <li v-on:click="toggleShow" class="launch list-group-item list-group-item-action flex-column align-items-start">
    
      <div class="d-flex flex-row h-100 justify-content-between">      
        <h5>{{ info.name }}</h5> 
        <h5 class="text-right">{{countdown}}</h5>        
      </div>
      <div class="d-flex flex-row h-100 justify-content-between"> 
          <a><font-awesome-icon icon="map-marker-alt"/> {{info.location}}</a> 
          <a class="text-right"> {{info.net}}</a>           
      </div>    

      <div v-if="show" class="description">
        <div class="container">
         
          <table class="table">
            
               <tr v-if="info.missionDescription">
                <th>Mission</th>
                <td>{{info.missionDescription}}</td>
              </tr>
              <tr v-if="info.windowStart">
                <th>Launch Window</th>
                <td>{{info.windowStart}} - {{info.windowEnd}}</td>
              </tr>
              <tr v-if="info.agency">
                <th>Agency</th>
                <td>{{info.agency}}</td>
              </tr>
              <tr v-if="info.vidurls[0]">
                <th>Stream</th>
                <td><a class="streamUrl" :href="info.vidurls[0]" target="_blank"> {{info.vidurls[0]}}</a></td>
              </tr>
            
          </table>
        </div>
      </div>
    
  </li>  
</template>

<script>

export default {
  name: 'Launch',
  props: {
    info: Object
  },
  data() {
    return {
      ticker: null,
      countdown: "",
      show: false
    }
  },
  mounted() {
    
    if(Number(this.info.netstamp) <= 0) {
      this.countdown = "TBD"
    } else {
      this.ticker = setInterval(this.updateCountDown, 1000)
    }
    
  },
  destroyed() {
    clearInterval(this.ticker)
  },
  methods: {
    updateCountDown: function() {
      
      let timeDiffSeconds = Number(this.info.netstamp) - Math.floor(Date.now() / 1000); 
      if(timeDiffSeconds < 0) {
        this.countdown = "LIFT-OFF!"
        
      } else {
        let days = Math.floor(timeDiffSeconds / 60 / 60 / 24)
        let hours = Math.floor(timeDiffSeconds / 60 / 60) - days * 24
        let minutes = Math.floor(timeDiffSeconds / 60) - days * 24 * 60 - hours * 60
        let seconds = timeDiffSeconds - days * 24 * 3600 - hours * 3600 - minutes * 60
        this.countdown = `${days}:${hours.toString().padStart(2,'0')}:${minutes.toString().padStart(2,'0')}:${seconds.toString().padStart(2,'0')}`
      }    
      

    },
    toggleShow: function() {
      this.show = !this.show;
    }

    
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.launch {
  padding: 15px 10px;
  cursor: pointer;
  
}

.text-right {
  text-align: right; 
}
.description {
  margin-top: 15px;
}
.streamUrl {
  color: rgb(3, 3, 117);
}
.streamUrl:hover {
  color: rgb(63, 63, 236);
}
</style>
