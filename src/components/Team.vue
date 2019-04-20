<template>
  <div class="site-container">

    <header>
      <div class="container center">
         <h1 class="site-title">HELLO TEAM</h1>
         <div class="center">
             <input class="search-box" type="text" placeholder="Search Name, department, job title or state" v-model="search">
         </div>
      </div>
    </header>
    

    <div class="container">

      <div class="all-team-members"  v-if="results">

          <div v-for="result in filteredResults">

            <div v-on:click="toggleItem" class="team-member">

              <div class="image">
                <img v-bind:src="result.headshot.large" v-bind:alt="result.name.first + ' ' + result.name.last">
              </div>
              <h4 class="name"> {{ result.name.first + ' ' + result.name.last}}</h4>
              <div class="job-title"> {{ result.job_title }}</div>
              <div class="location">{{ result.location.state}}</div>
              <div class="location">{{ result.isActive }}</div>

            </div>

            <div v-show="toggled" class="lightbox">
              <div class="image">
                  <img v-bind:src="result.headshot.large" v-bind:alt="result.name.first + ' ' + result.name.last">
              </div>
              <h4 class="name"> {{ result.name.first + ' ' + result.name.last}}</h4>
              <div class="job-title"><span>Manager Icon</span> {{ result.job_title }}</div>
              <div class="department"> {{ result.department }}</div>
              <div class="five-year-staff">Five year</div>
              <div class="phone">{{ result.phone }}</div>
              <div class="address">
                <div class="street">{{ result.location.street }}</div>
                <div class="city-state-zip">{{ result.location.city + ', ' + result.location.state + ' ' + result.location.postcode}}</div>
              </div>
              <div class="google-link">
                <a v-bind:href="'https://www.google.com/maps/?q=' + result.location.street + ' ' + result.location.city + ', ' + result.location.state + ' ' + result.location.postcode">Google Map</a>
              </div>
              <div class="local-time">Local Time</div>
              <div class="skills">Skills</div>   
            </div>

          </div>
      </div>

    </div>

  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'team',
  data () {
    return {
      results: '',
      search: '',
      toggled: false
    }
  },
  methods: {
    getResult(){
      axios.get('https://codepen.io/guanyixi/pen/VNxYwx.js').then(response => {
        this.results = response.data.results;
        // this.results.forEach(function(result){
        //   result.isActive = false;
        // });
      });
    },
    toggleItem: function() {
      this.toggled = !this.toggled;
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
  max-width:1100px;
  margin:0 auto;
}

.center{
  text-align:center;
}

header{
  padding:15px 0 20px;
}

.site-title{
  font-weight:300;
  font-size:2.6em;
}

.search-box{
  margin:0 auto;
  max-width:100%;
  width:400px;
}

.all-team-members{
  display:flex;
  flex-wrap:wrap;
  justify-content: center;
}

.all-team-members > div{
  width:20%;
}

.team-member{
  text-align:center;
  padding:20px 10px;
  cursor: pointer;
}

.team-member:hover img{
  filter:grayscale(0);
  transition: all 0.5s;
  box-shadow:0 0 15px rgba(0, 0, 0, 0.3);
}
.team-member:hover .name{
  color:#4E9A92;
}

.team-member img{
  border-radius:50%;
  filter: grayscale(100%);
}

.name{
  margin-top:10px;
  text-transform: uppercase;
  font-size:0.9em; 
  font-weight:900;
  transition: color 0.5s;
}

.job-title, .location{
  font-size:0.9em;
}

.job-title{
  margin-bottom:10px;
}

.location{
  color:#666;
}

/* .lightbox{
  transform:scale(0);
  transition:all 0.7s;
  width:0;
  height:0;
} */



</style>
