<template>
  <div class="launch-list">
    <div v-if="error" class="error">
      <center>
        <h3>We are having trouble getting data! Please try again later!</h3>
      </center>
    </div>
    <div v-else class="success">
      <div v-if="loading" class="loading">
        <div class="spinner-border" role="status">
          <span class="sr-only">Loading...</span>
        </div>
      </div>
      <div v-else class="loaded">
        <ul class="list-group list-group-flush">
          <launch
            v-for="l in launches"
            v-bind:key="l.id"
            v-bind:info="l"
          ></launch>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import Launch from "../components/Launch.vue";
import axios from "axios";
export default {
  name: "LaunchList",
  components: {
    Launch,
  },
  data() {
    return {
      launches: null,
      error: false,
      loading: true,
    };
  },
  mounted() {
    axios
      .get(
        "https://ll.thespacedevs.com/2.0.0/launch/upcoming/?limit=30&mode=detailed"
      )
      .then((response) => {
        this.launches = response.data.results.map((x) => {
          return {
            id: x.id,
            name: x.name,
            net: x.net,
            status: x.status.name,
            windowStart: x.window_start,
            windowEnd: x.window_end,
            location: x.pad.location.name,
            mission: x.mission ? x.mission.name : "",
            missionDescription: x.mission ? x.mission.description : "",
            rocket: x.rocket.name,
            agency: x.launch_service_provider.name,
            vidurls: x.vidURLs,
          };
        });
      })
      .catch((err) => {
        console.log(err);
        this.error = true;
        this.$emit("error");
      })
      .finally(() => {
        this.loading = false;
      });
  },
};
</script>

<style scoped>
.launch-list {
  margin-top: 2em;
  margin-bottom: 2em;
}
.loading {
  text-align: center;
  margin-top: 4em;
}
.error {
  margin-top: 4em;
}
</style>
