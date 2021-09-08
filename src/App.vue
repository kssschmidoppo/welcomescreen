<template>
  <div id="app">
    <!-- title -->
    <h1> {{title}} </h1>
    <span id="time">{{currentDate}}</span>

    <!-- entry list -->
      <ul class="entry">
        <li class="desktop" v-for="entry in filteredEntries" :key="entry.id">
            <span class="timer">{{entry[0]}} Uhr {{entry[1].replaceAll("/",".")}}</span>
            <br>
            <h3 class="info">{{entry[2]}} </h3>
            <br>
            <span class="description">{{entry[3]}}  </span>
            <br>
        </li>
      </ul>


    <!-- footer -->
    <footer>
      <img src="./assets/Opportunity.png"> 
      <img src="./assets/SAG_Logo_De.png">
      <img src="./assets/STZH_SEB_Logo.png">
    </footer>        
  </div><!--App-->
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data () {
    return {
      title: "Welcome to Opportunity",
      currentDate:"",
      gsheet_url:
      "https://sheets.googleapis.com/v4/spreadsheets/1qQqe7saCzl9r2zkXJlseq76PRiWHa0SWgxZbsSjFmwY/values:batchGet?ranges=A1%3AE100&valueRenderOption=FORMATTED_VALUE&key=AIzaSyDqxstLQi32iJXW7G8tjKNZnz_KiFv7srQ",
      entries: [],
    };
  },

  computed: {
    filteredEntries(){
      return[...this.entries].slice(1);
    }

  },
  methods: {
    getData() {
      axios.get(this.gsheet_url).then((response) => {
        this.entries = response.data.valueRanges[0].values;
        console.log(response);
      });
    },
    updateCurrentDate(){
      let today = new Date();
      const date = `${today.getDate()}.${today.getMonth() +1}.${today.getFullYear()}`;
      this.currentDate = date;
    },
    refreshData(){
      this.getData();
      this.updateCurrentDate();
    }
  }, 
  mounted(){
    this.refreshData(); //load initial data

    setInterval(() => {
      this.refreshData();
    }, 18000000) // updates every 30 minutes
  }      
};

</script>

<style lang="scss">

@import url(https://fonts.googleapis.com/css?family=Inter:wght@500;900&display=swap);

#app {

  background:#E8EFF4;;
  font-family: "Inter", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #0f3e6d;
  margin-top: 60px;
  
  h1{
    font-weight: 900;
    font-size:62px;
    line-height:75px;
    margin:80px 0 20px 0;
    color:#323D4A;
    } 
    #time { 
        font-weight: 500;
        font-size: 62px;
        color:#9aa7b1;
        margin:0;     
      } 
      .entry{
        padding-left:0;
      }
      .desktop{
        padding: 35px 40px;
        margin:40px 0;
        font-size:28px;
        line-height:1.3;
        list-style:none;
        background: #0F05A0; 
        .timer{
        font-weight: 900;
        font-size: 28px;
        color: #EB5E00;
        }
        .info{
          font-size: inherit;
          font-weight: 900;
          margin:0;
          color: #ffbfab;
        }
        .description{
          font-weight:500;
          color:#ffbfab;
        }  
      }
        footer{
          display: flex-wrap;
          position: fixed;
          box-sizing: border-box;
          justify-content: space-between;
          padding:40px;
          bottom:0;
          width: 100%;
          left: 0;
          background: #FFFFFF
        }
        img {
            height:50px
            }

}
@media screen and (min-width: 480px) {
  
  #main {margin-left: 216px;}
}
</style>
