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

*{
  box-sizing: border-box;
}

body{
  margin:0;
  font-size:15px;
   font-family: 'Lato', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  line-height:1.2;
}

h1, h2, h3, h4, h5, h6{
  margin:0;
  padding:0 0 0.3em 0;
}

p{
  margin:0;
  padding:0 0 0.7em;
}

a{
  color:#4E9A92;
  text-decoration: none;
  transition: all 0.5s;
}

a:hover{
  text-decoration: underline;
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

button{
  border:0;
  border-radius:0;
  transition: all 0.5s;
  cursor: pointer;
}

button:active, button:focus, button:visited{
    outline:0;
    opacity:0.8;
}
</style>
