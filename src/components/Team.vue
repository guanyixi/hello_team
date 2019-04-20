<template>
  <div>

    <header>
      <div class="container center">
         <div class="logo"><img src="../assets/logo.svg" alt="Hello Team Logo"></div>
         <div class="center">
             <input class="search-box" type="text" placeholder="Search Name, department, job title or state" v-model="search">
         </div>
      </div>
    </header>
    

    <main class="container">
      <div class="all-team-members"  v-if="results">
          <div v-for="result in filteredResults">
             <member :result="result"></member>
          </div>
      </div>
    </main>

    <footer>
      <div class="container center">
        <a title="View repository" target="_blank" class="github" href="#"><i class="fab fa-github-alt"></i></a>
        <button title="Back to top" class="back-to-top"><i class="fas fa-chevron-up"></i></button>
      </div>
    </footer>

  </div>
</template>

<script>
import Member from './Member'
import axios from 'axios';
export default {
  components:{
    'member': Member
  },
  name: 'team',
  data () {
    return {
      results: '',
      search: '',
    }
  },
  methods: {
    getResult(){
      axios.get('https://codepen.io/guanyixi/pen/VNxYwx.js').then(response => {
        this.results = response.data.results;
      });
    }
  },
  beforeMount(){
    this.getResult();
  },
  computed: {
    filteredResults: function(){
      return this.results.filter((result) => {
        const fullName = result.name.first + result.name.last;
        const stateFull = result.location.state;
        const jobTitle = result.job_title;
        const department = result.department;
        const searchBase = fullName + stateFull + department + jobTitle;
        return searchBase.toLowerCase().match(this.search.toLowerCase());
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.container{
  max-width:1500px;
  margin:0 auto;
  padding:0 20px;
}

.center{
  text-align:center;
}

header{
  padding:15px 0 20px;
}

.logo img{
  width:240px;
}

.search-box{
  margin:10px auto 0;
  max-width:100%;
  width:400px;
}

.all-team-members{
  display:flex;
  flex-wrap:wrap;
  justify-content: center;
}

.all-team-members > div{
  width:180px;
}

footer{
  padding:30px 0 20px;
}

.github, .back-to-top{
  display:inline-block;
  vertical-align: top;
  padding:10px 13px;
  font-size:1.8em;
  color:#4E9A92;
  opacity:0.7;
}

.github:hover, .back-to-top:hover{
  opacity: 1;
}

</style>
