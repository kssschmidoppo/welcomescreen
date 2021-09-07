/* eslint-disable vue/require-v-for-key */
<template>
  <div id="app">
    <div id="desktop">
      <h1> {{title}} </h1>
     <!-- <input id="time" type="date" v-model="date"> -->
    <span class="time">{{currentDate}}</span>

          <!--<template v-for="(display, display.id ) in desktop" :key=''> 
            <h3> {{ display.arriver }} </h3>
            <p> {{display.info}} </p>
            <p> {{dispaly.description}} </p>
          </template>  --> <!--rectangleOne--> 
          <ul >
        <li class="deskstop" v-for="entry in filteredEntries" :key="entry.id">
            <h3 class="info"> {{entry[2]}} besi </h3>
            <span class="time">{{entry[0]}} Uhr, {{entry[1].replaceAll("/",".")}} </span>
            <br>
            <span class="description"> {{entry[3]}}Bliblablubbb </span>
            <br>
        </li>
      </ul>
      <footer>
        <img src="./assets/Opportunity.png"> 
        <img src="./assets/SAG_Logo_De.png">
        <img src="./assets/STZH_SEB_Logo.png">
           <!-- <ul id="footer" >
              <li v-for="(item, index) in footerItems" :key='index'> {{ item.image_path }}-->
                  <!-- <img class="image" :src="item.image_path"> -->
          <!--    </li> -->
          <!--  </ul>  --><!--footer-->    
      </footer>        
    </div><!--destop-->
  </div><!--App-->
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data () {
    return {
      title: "Welcome to Opportunity",
     // date: 
        //new Date().toISOString().substr(0, 10),
        currentDate:"",
        gsheet_url:
        "https://sheets.googleapis.com/v4/spreadsheets/15_C4Rx7692L5WNy4y4DEksZKDM0HRjrcUuXVTMtElwI/values:batchGet?ranges=A1%3AE100&valueRenderOption=FORMATTED_VALUE&key=AIzaSyBesotaNgSaTUIhrSKjEaExdi-ksKInhoE",
        entries: [],
        //desktop: {
          //arriver: "14:00 Uhr",
          //info: "Basisbeschäftigung Besuch",
          //description:"Interessierte für den zweiten Kurs werden uns besuchen"
        //},
        //item:"",  
        //footerItems: [{
         // image_path:"./assets/Opportunity.png"
       // },{
        //  image_path:"./assets/SAG_Logo_De.png"
       // },{
       // image_path:"./assets/STZH_SEB_Logo.png"      //  }]
   
}
  
},

methods:{
  getData(){
    axios.get(this.gsheet_url).then((response) =>{
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
mounthed(){
  this.refreshData();
  setInterval(() => {
    this.refreshData();
  }, 18000000) // updates every 30 minutes
}      
};

</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Inter:wght@500;900&display=swap");
#app {
  background:#E5E5E5;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #0f3e6d;
  margin-top: 60px;
  #desktop{
    height: 1920px;
    width: 1080px;
    margin: auto;
    background: #E8EFF4;
    h1{
      font-family:Inter;
      font-weight: 900;
      font-size:62px;
      line-height:75px;
      color:#323D4A;
      margin:80px 0 20px 0;
      }  

    }
    #time { 
          font-family: Inter;
          font-style:normal;
          font-weight: 500;
          font-size: 62px;
          line-height: 75px;
          color:#9aa7b1;
          margin:0;
        }
        .rectangleOne{
          padding: 35px 40px;
          margin:40px;
            position: auto;
            height: 182px;
            width: 919px;
            font-size:28px;
            list-style:none;
            background: #0F05A0;   
        }
        h3{
          padding:4.35%, 13.49%,20.33%;  
          font-family: Inter;
          font-style: normal;
          font-weight: 900;
          font-size: 28px;
          line-height: 36px;
          color: #EB5E00;
            }
            .info{
              font-size: inherit;
              font-weight: 900;
              font-style: bold;
              color: #ffbfab;
            }
            .discription{
              font-weight:500;
              color:#ffbfab;
            }
        
   footer{
    display: flex;
    position: fixed;
    display: inline;
    box-sizing: border-box;
    justify-content: space-between;
    padding:40px;
    bottom:0;
    padding:30;
    width: 100%;
    height: 130px;
    left: 0px;
    top: 1790px;
    background: #FFFFFF
    img{
      height:50px
    }
  }        


}

</style>
