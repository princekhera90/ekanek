<template>
  <div id="app">
    <input v-model="searchTerm" @keydown.enter="search" class="search_input" placeholder="Enter Free Text"/>
    <br />
    <button  class="seach_button" @click="search">Search</button>
    <div>
      <div>Number of Columns : 
        <button v-for="col in noOfColumnsArray" :key="col" :class="{'selected' : noOfColumn === col}" @click="colClick(col)">{{col}}</button>
      </div>
    </div>
    <div class="content" v-if="(data && data.length) || loading">
      <div v-if="loading">Loading...</div>
      <Card v-for="ele in data" :image="ele.image" :title="ele.title" :key="ele.id" v-else />
    </div>
    <div class="no_data" v-else>Please Type and Press Search to Search For Images</div>
  </div>
</template>

<script>
import Axios from 'axios'
import Card from './components/card.vue'

export default {
  name: 'App',
  components: {
    Card
  },
  data(){
    return{
      data : {},
      searchTerm : '',
      loading : false,
      noOfColumn : 2,
      noOfColumnsArray : [2,3,4]
    }
  },
  methods : {
    colClick(col){
      const content = document.getElementsByClassName('content')
      if(content && content.length > 0){
        const factor = Math.abs(this.noOfColumn-col)
        let width = Number(content[0].offsetWidth)
        if(this.noOfColumn > col)
          width-=200*factor
        else if(this.noOfColumn < col)
          width+=200*factor
        content[0].style.width = `${width}px`
      }
      this.noOfColumn = col
    },  
    async search(){
      try{
      this.loading = true
      await Axios.get(`https://api.flickr.com/services/rest/?method=flickr.photos.search&api_key=37e4000ad6c1758460ea9a8f5b1f187a&per_page=50&format=json&text=${this.searchTerm}`).then(res=>{
        this.data = JSON.parse(JSON.stringify( res.data.split('jsonFlickrApi(')[1]))
      })
      this.data = JSON.parse((this.data.slice(0,this.data.length-1))).photos.photo
      this.data.forEach(ele=>{
        ele.image = "https://api.flickr.com/services/rest/?method=flickr.photos.getSizes&api_key=37e4000ad6c1758460ea9a8f5b1f187a&photo_id="+ele.id+"&format=json";
      })
      this.loading = false
      this.colClick(this.noOfColumn)
      } catch{
        this.loading = false
      }
    }
  },
  async mounted(){
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  .search_input{
    margin : 10px;
    width : 200px;
    height : 20px;
  }
  .seach_button{
    margin : 10px;
    width : 80px;
    height : 35px;
  }

  .selected {
    background-color : green;
  }

  .content{
    align-items: center;
    margin-left : 36%;
    width : 600px;
    display : flex;
    flex-wrap: wrap;
  }
  .no_data{
    margin-top : 10%;
  }
}
</style>
