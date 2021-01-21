<template>
  <div id="app">
    <search :parent-Ubikestops="uBikeStops" @update="searchStop"></search>
    <ubike-table :parent-Ubikestops="searchStops" :page-Size="limit" :select-Page="currentPage"></ubike-table>
    <pagination :total-Page="totalPage" :select-Page="currentPage" @update="updatePage"></pagination>
  </div>
</template>

<script>
import Search from './components/Search.vue'
import UbikeTable from './components/UbikeTable.vue'
import Pagination from './components/Pagination.vue'

export default {
  name: 'App',
  components: {
    Search,
    UbikeTable,
    Pagination
  },
  data () {
    return {
      uBikeStops: [],
      searchStops: [],
      limit: 15, // 每頁顯示行數
      currentPage: 1, // 當前頁                    
    }
  },
  computed: {
    totalPage(){
      return Math.ceil(this.searchStops.length / this.limit);
    },
  },
  methods: {
    searchStop(stops, page){
      this.searchStops = stops;
      if (page > 0) {
        this.currentPage = page;
      }      
    },
    updatePage(page){
      this.currentPage = page;
    },    
  },  
  created () {
    fetch('https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.gz') //fetch:非同步的動作
        .then(res => res.json())
        .then(res => {
          // 將 json 轉陣列後存入 this.uBikeStops
          this.uBikeStops = Object.keys(res.retVal).map(key => res.retVal[key]);
      });            
  }  
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 10px;
}
    body {
      padding: 1em;
    }
    .aa {
      cursor: pointer;
    }
    .fa {
      color: #AAA;
    }
    .sortColor {
      color: rgb(56, 56, 56);
    }

    ul.pagination {
        display: inline-block;
        padding: 0;
        margin: 0;
    }
    ul.pagination li {display: inline;}
    ul.pagination li a {
        color: black;
        float: left;
        padding: 8px 16px;
        text-decoration: none;
        transition: background-color .3s;
        border: 1px solid #ddd;
    }
    ul.pagination li a.active {
        background-color: #4CAF50;
        color: white;
        border: 1px solid #4CAF50;
    }
    ul.pagination li a:hover:not(.active) {background-color: #ddd;}
</style>
