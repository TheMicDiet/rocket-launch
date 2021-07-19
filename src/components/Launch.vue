<template>
  <li
    v-on:click="toggleShow"
    class="launch list-group-item list-group-item-action flex-column align-items-start"
  >
    <div class="d-flex flex-row justify-content-between">
      <h5>{{ info.name }}</h5>
      <h5 class="text-right">{{countdown}}</h5>
    </div>
    <div class="d-flex flex-row justify-content-between">
      <a>
        <font-awesome-icon icon="map-marker-alt"/>
        {{info.location}}
      </a>
      <a class="text-right">{{netFormatted}}</a>
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
            <td>{{windowStartFormatted}} - {{windowEndFormatted}}</td>
          </tr>
          <tr v-if="info.agency">
            <th>Agency</th>
            <td>{{info.agency}}</td>
          </tr>
          <tr v-if="info.vidurls[0]">
            <th>Stream</th>
            <td>
              <a class="streamUrl" :href="info.vidurls[0].url" target="_blank">{{info.vidurls[0].url}}</a>
            </td>
          </tr>
        </table>
      </div>
    </div>
  </li>
</template>

<script>
import moment from 'moment'
export default {
  name: "Launch",
  props: {
    info: Object
  },
  data() {
    return {
      ticker: null,
      countdown: "",
      show: false
    };
  },
  mounted() {
    if (this.info.status === "TBD") {
      this.countdown = "TBD";
    } else {
      this.ticker = setInterval(this.updateCountDown, 1000);
    }
  },
  destroyed() {
    clearInterval(this.ticker);
  },
  computed: {
    netFormatted: function(){
      return this.dateFormatted(this.info.net);
    },
    windowStartFormatted: function() {
      return this.dateFormatted(this.info.windowStart);
    },
    windowEndFormatted: function() {
      return this.dateFormatted(this.info.windowEnd);
    }
  
  },
  methods: {
    updateCountDown: function() {
      const netstamp = Date.parse(this.info.net);
      let timeDiffSeconds = Math.floor((netstamp - Date.now()) / 1000);
      if (timeDiffSeconds < 0) {
        this.countdown = "LIFT-OFF!";
      } else {
        let days = Math.floor(timeDiffSeconds / 60 / 60 / 24);
        let hours = Math.floor(timeDiffSeconds / 60 / 60) - days * 24;
        let minutes =
          Math.floor(timeDiffSeconds / 60) - days * 24 * 60 - hours * 60;
        let seconds =
          timeDiffSeconds - days * 24 * 3600 - hours * 3600 - minutes * 60;
        this.countdown = `${days}:${hours
          .toString()
          .padStart(2, "0")}:${minutes
          .toString()
          .padStart(2, "0")}:${seconds.toString().padStart(2, "0")}`;
      }
    },
    toggleShow: function() {
      this.show = !this.show;
    },
    dateFormatted(date) {
      return moment(date).format("DD.MM.yyyy HH:mm:ss")
    }
  }
};
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
.launch {
  border-image: linear-gradient(
    to right,
    rgba(3, 83, 158, 0.5) 0%,
    rgba(32, 124, 229, 0.5) 100%
  );
  border-image-slice: 1;
  border-width: 1px;
}
</style>
