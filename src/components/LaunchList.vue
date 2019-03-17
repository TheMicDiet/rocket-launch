<template>
    <div class="launch-list">
        <div v-if="error" class="error">
            <center><h3>We are having trouble getting data! Please try again later!</h3></center>
            
        </div>
        <div v-else class="success">
            <div v-if="loading" class="loading">
                <div class="spinner-border" role="status">
                    <span class="sr-only">Loading...</span>
                </div>
            </div>
            <div v-else class="loaded">
                <ul class="list-group list-group-flush">
                 <launch v-for="l in launches" v-bind:key="l.id" v-bind:info="l"></launch>
                </ul>
            </div>
           
        </div>
        
    </div>
</template>

<script>
import Launch from "../components/Launch.vue"
import axios from "axios"
export default {
    name: "LaunchList",
    components: {
        Launch
    },
    data() {
        return {
            launches: null,
            error: false,
            loading: true
        }        
    },
    mounted() {
        axios.get("https://launchlibrary.net/1.4/launch?mode=verbose&next=30").then(
            response => {
                this.launches = response.data.launches.map(x => {
                    return {
                        id: x.id,
                        name: x.name,
                        net: x.net,
                        tbd: x.tbddate + x.tbdtime,
                        windowStart: x.windowstart,
                        windowEnd: x.windowend,
                        location: x.location.pads[0].name,
                        mission: x.missions.length > 0 ? x.missions[0].name : "",
                        missionDescription: x.missions.length > 0 ? x.missions[0].description : "",
                        rocket: x.rocket.name,
                        agency: (x.missions) ? ((x.missions[0]) ? (x.missions[0].agencies ? (x.missions[0].agencies[0] ? x.missions[0].agencies[0].name : ""): "") : "") : "",
                        netstamp: x.netstamp,
                        vidurls: x.vidURLs
    
                    }
                })
                
                }
        ).catch(() => {
           this.error = true
           this.$emit('error')  
        }).finally(() => {this.loading = false}) 
        
    },
    
}
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
