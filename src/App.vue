<template>
  <div id="app">    
    <custom-header title="Launches"></custom-header>
    <div class="modal fade" id="about" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
      <div class="modal-dialog modal-dialog-centered" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">About</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <p>Powered by Vue.js and Bootstrap. Data from <a id="ref" href="https://launchlibrary.net">Launch Library</a>.</p>
            
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-dark" data-dismiss="modal">Close</button>            
          </div>
        </div>
      </div>
    </div>
    <div class="container pb-2 pt-2" v-on:error="errorOccured = true">    
      <launch-list></launch-list>
    </div>
    <div v-bind:class="{'fixed-bottom': errorOccured}" class="footer text-light bg-dark">
      <a href="http://mi-dietrich.de">&copy;2019 by Michael Dietrich</a>
    </div>
  </div>
</template>

<script>
import LaunchList from './components/LaunchList.vue'
import CustomHeader from './components/CustomHeader.vue'


export default {
  name: 'app',
  components: {
    LaunchList,
    CustomHeader
  },
  data() {
    return {
      errorOccured: false,
    }
    
  },
  methods: {
    toggleFixedFooter: function() {
      this.errorOccured = !this.errorOccured;
      
    }
  }
}
</script>

<style lang="scss">
  $dark: rgba(3,83,158,1);
  $light: #FFFFFF;
  $body-bg: #FFFFFF;

  @import "~bootstrap/scss/bootstrap";  // import complete bootstrap.scss source from node_modules using ~ alias
  @import url('https://fonts.googleapis.com/css?family=Oxygen');
  h1, h2, h3, h4, h5, p, a {
    font-family: 'Oxygen', sans-serif;
  }
  html {
    position: relative;
    min-height: 100%;
  }
  body {
    margin-bottom: 60px; /* Margin bottom by footer height */
  }
  .footer {
    text-align: center;
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 60px; /* Set the fixed height of the footer here */
    line-height: 60px; /* Vertically center the text there */
    background: rgba(3,83,158,1);
    //background-image: linear-gradient(to left, #0575e6, #0b5eca, #0b47af, #083194, #021b79);
    background: linear-gradient(to right, rgba(3,83,158,1) 0%, rgba(32,124,229,1) 100%);
  }
  .footer a {
    color: $light;
    text-decoration: none;
  }
  a:hover {
    color: rgb(234, 234, 234);
  }
  #ref {
    color: rgb(3, 3, 117);
  }
  #ref:hover {
    
    color: rgb(63, 63, 236);
  }
  

</style>
