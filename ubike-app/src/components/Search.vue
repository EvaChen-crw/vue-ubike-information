<template>
    <p>
      站點名稱搜尋: <input type="text" v-model="searchName">
    </p>
</template>

<script>
export default {
  props:{
    parentUbikestops: {
      type: Array,
      required: true
    }
  },
  data () {
    return {
      searchName: '',
      currentPage: 0,  
    }
  },
  watch: {
    searchStopName(){
      if (this.searchName !== ''){
        this.currentPage = 1;
      }      
      this.$emit("update", this.searchStopName, this.currentPage);
    }
  },
  computed: {
    searchStopName(){
      return this.parentUbikestops.filter(u => u.sna.toLowerCase().includes(this.searchName));
    },
  },
}
</script>
