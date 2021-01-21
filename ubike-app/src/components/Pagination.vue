<template>    
    <div style="text-align: center;">
      <ul class="pagination">
        <li :class="{'disabled': (selectPage === 1)}"
          @click="setPage(selectPage-1)"><a href="#">Prev</a></li>
        <li v-for="(n, index) in totalPage" :key="index" @click="setPage(n)">
          <a href="#" :class="{'active': (selectPage === (n))}">{{n}}</a>
        </li>
        <li :class="{'disabled': (selectPage === totalPage)}"
           @click="setPage(selectPage+1)"><a href="#">Next</a></li>
      </ul>
    </div>
</template>

<script>
export default {
  props:{
    totalPage: {
      type: Number,
      required: true
    },
    selectPage:{
      type: Number,
      required: true      
    },    
  },
  data () {
    return {
      currentPage: this.selectPage,        
    }
  },
  methods: {
    setPage(page){
      if( page <= 0 || page > this.totalPage ){
        return;
      }
      this.currentPage = page;
      this.$emit("update", page);
    }  
  }
}
</script>
