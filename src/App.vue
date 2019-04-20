<template>
  <div id="app">
    <team></team>
    
  </div>
</template>

<script>
import Team from './components/Team'

export default {
  name: 'App',
  components: {
    Team
  }
}
</script>

<style>

body{
  margin:0;
  font-size:16px;
   font-family: 'Lato', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

h1, h2, h3, h4, h5, h6{
  margin:0;
  padding:0 0 0.3em 0;
}

input{
  background-color:transparent;
  border:1px solid #ccc;
  border-radius:40px;
  padding:15px 20px;
  text-align:center;
  font-size:16px;
}

input:focus{
  outline:0;
}
</style>
