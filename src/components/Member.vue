<template>

    <div class="member">

        <div v-on:click="toggleItem" class="team-member">
            <div class="image">
            <img v-bind:src="result.headshot.large" v-bind:alt="result.name.first + ' ' + result.name.last">
            </div>
            <h4 class="name"> {{ result.name.first + ' ' + result.name.last}}</h4>
            <div class="job-title"> {{ result.job_title }}</div>
        </div>

        <div v-show="toggled" class="lightbox-container">
            <div class="lightbox">
                <button class="close-lightbox" v-on:click="toggleItem"><i class="fa fa-times"></i></button>
                <header>
                    <div class="image">
                        <img v-bind:src="result.headshot.large" v-bind:alt="result.name.first + ' ' + result.name.last">
                    </div>
                    <div class="member-icons">
                        <span title="Manager" v-if="result.is_manager"><i class="fas fa-users"></i></span>
                        <span title="10 years at company" v-if="isFiveYear()"><i class="fas fa-star"></i></span>
                    </div>
                </header>
                <main>
                    <h1 class="name">{{ result.name.first + ' ' + result.name.last}}</h1>
                    <div class="job-title"><span>Manager Icon</span> {{ result.job_title }}</div>
                    <div class="department"><b>{{ result.department }}</b></div>
                    <p class="phone">
                        <b><i class="fas fa-mobile-alt"></i></b>
                        <a v-bind:href=" 'tel:' + result.phone ">{{ result.phone }}</a>
                        <i class="local-time">{{ result.name.first }}'s time: <span>{{localTime}}</span></i>
                    </p>
                    <p class="address">
                        <b><i class="fas fa-map-marker-alt"></i></b>
                        <a target="_blank" v-bind:href="'https://www.google.com/maps/?q=' + result.location.street + ' ' + result.location.city + ', ' + result.location.state + ' ' + result.location.postcode">
                            {{ result.location.street + ' ' + result.location.city + ', ' + result.location.state + ' ' + result.location.postcode }}
                        </a>
                    </p>
                    <div v-if="skills.length != 0">
                        <p class="skills">
                            <b>Skills: </b>
                            <span v-for="skill in skills">{{skill.id}}</span>
                        </p> 
                    </div>  
                </main>
            </div>
        </div>

    </div>

</template>

<script>

export default {
  name: 'member',
  props: {
      result: Object
  },
  data () {
    return {
      toggled: false,
      localTime: '',
      skills: this.result.skills
    }
  },
  methods: {
    toggleItem(){
      this.toggled = !this.toggled;
    },
    calcTime(){
        var offset = this.result.location.timezone.offset;
        offset = offset.replace(':', '');
        var d = new Date();
        var utc = d.getTime() + (d.getTimezoneOffset() * 60000);
        var date = new Date(utc + (3600000*offset));
        var hours = date.getHours();
        var minutes = date.getMinutes();
        var ampm = hours >= 12 ? 'pm' : 'am';
        hours = hours % 12;
        hours = hours ? hours : 12; // the hour '0' should be '12'
        minutes = minutes < 10 ? '0'+minutes : minutes;
        var strTime = hours + ':' + minutes + ' ' + ampm;
        this.localTime = strTime;
    },
    isFiveYear(){
        // start date in yyyymmdd
        var userDate = this.result.date_started;
        userDate = new Date(userDate);
        var y = userDate.getFullYear();
        var m = userDate.getMonth() + 1;
        var d = userDate.getDate();

        // converting the integer values we got above to strings
        y = y.toString();
        m = m.toString();
        d = d.toString();

        // making days or months always 2 digits
        if (m.length === 1) {
            m = '0' + m;
        }
        if (d.length === 1) {
            d = '0' + d;
        }
        // Combine the 3 strings together
        userDate = y + m + d;

        // today in yyyymmdd
        var x = new Date();
        var y = x.getFullYear().toString();
        var m = (x.getMonth() + 1).toString();
        var d = x.getDate().toString();
        (d.length == 1) && (d = '0' + d);
        (m.length == 1) && (m = '0' + m);
        var today = y + m + d;
        return (today - userDate) > 100000;
    }
  },
  beforeMount(){
    this.calcTime();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

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

.team-member .name{
  margin-top:10px;
  text-transform: uppercase;
  font-size:0.9em; 
  font-weight:900;
  transition: color 0.5s;
}

.team-member .job-title, .team-member .location{
  font-size:0.9em;
}

.team-member .job-title{
  margin-bottom:10px;
}

.team-member .location{
  color:#666;
}

.lightbox-container{
    position:fixed;
    z-index:100;
    top:0;
    left:0;
    width:100%;
    height:100%;
    background-color:rgba(0, 0, 0, 0.5);
    display:flex;
    justify-content: center;
    align-content: center;
    flex-direction:column;
}

.lightbox{
    position:relative;
    width:700px;
    max-width:90%;
    max-height:90%;
    background-color:white;
    margin:auto;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
    display:flex;
}

.close-lightbox{
    position:absolute;
    right:10px;
    top:10px;
    font-size:1.3em;
    color:#4E9A92;
    opacity: 0.7;
}

.close-lightbox:hover{
    opacity:1;
}

.close-lightbox:hover{
    color:#4E9A92;
}

.lightbox header{
    background-color:#4E9A92;
    padding:45px 30px 20px;
    width:30%;
    text-align:center;
}

.lightbox .image img{
    border-radius:50%;
    margin-bottom:10px;
}

.member-icons i{
    color:white;
    opacity: 0.5;
    font-size:1.3em;
    padding:5px;
}

.lightbox main{
    padding:40px 40px 60px;
    width:70%;
    overflow-y:auto;
}

.lightbox h1{
    padding-bottom: 5px;
}

.lightbox .job-title{
    margin-bottom:3px;
}

.lightbox .department{
    margin-bottom:25px;
}

.lightbox main .fas{
    width: 15px;
}

.local-time{
    color:#999;
    padding-left:5px;
}

.local-time span{
    text-transform: uppercase;
}

.skills span:not(:last-child):after{
   content: ", ";
}

</style>
