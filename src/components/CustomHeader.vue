<template>
  
    <nav class="navbar navbar-dark bg-dark">
      <a class="navbar-brand" href="#">
        <font-awesome-icon icon="rocket"/> {{title}}
      </a>
      <a class="nav-item nav-link" href="" data-toggle="modal" data-target="#about"><font-awesome-icon icon="info-circle"/></a>
      
    </nav>
 

</template>

<script>


export default {
  
  name: 'CustomHeader',
  props: {
      title: String
  }
}
</script>


<style scoped>
@import url('https://fonts.googleapis.com/css?family=Righteous');
a {
  font-family: 'Righteous', cursive;
  font-size: 1.6rem;
  color: #F9F9FA;
}
a:hover {
  color: rgb(234, 234, 234);

}
</style>
