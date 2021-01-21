<template>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>#</th>
          <th>No.</th>
          <th>站點名稱</th>
          <th>場站區域</th>
          <th class="aa" @click="sortStop('sbi')">目前可用車輛            
            <i class="pull-right fa fa-sort-amount-asc sortColor" aria-hidden="true" :class="{ 'inverse': isReverse }" v-if="sortType == 'sbi' && isReverse"></i>            
            <i class="pull-right fa fa-sort-amount-desc sortColor" aria-hidden="true" :class="{ 'inverse': isReverse }" v-else-if="sortType == 'sbi' && !isReverse"></i>  
            <i class="pull-right fa fa-sort" aria-hidden="true" v-else></i>          
          </th>
          <th class="aa" @click="sortStop('tot')">總停車格            
            <i class="pull-right fa fa-sort-amount-asc sortColor" aria-hidden="true" :class="{ 'inverse': isReverse }" v-if="sortType == 'tot' && isReverse"></i>            
            <i class="pull-right fa fa-sort-amount-desc sortColor" aria-hidden="true" :class="{ 'inverse': isReverse }" v-else-if="sortType == 'tot' && !isReverse"></i>    
            <i class="pull-right fa fa-sort" aria-hidden="true" v-else></i>
          </th>
          <th>資料更新時間</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(s, index) in bikeStops.slice(pageStart, pageStart + limit)" :key="index">
          <td>{{ (selectPage-1) * limit + index + 1 }}</td> <!-- <= 取代這兒 -->
          <td>{{ s.sno }}</td>
          <td>{{ s.sna }}</td>
          <td>{{ s.sarea }}</td>
          <td>{{ s.sbi }}</td>
          <td>{{ s.tot }}</td>
          <td>{{ timeFormat(s.mday) }}</td>          
        </tr>
      </tbody>
    </table>
</template>

<script>
export default {
  props:{
    parentUbikestops: {
      type: Array,
      required: true
    },
    pageSize:{
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
      sortType: '',
      isReverse: true,
      limit: this.pageSize,
      cup: this.selectPage, //老師, 我想請問一下, 為何我用 this.cup這個值取代 this.selectPage, 資料就不會動了??
    }
  },
  computed:{
    bikeStops(){
      const stops = this.parentUbikestops;
      if(this.isReverse){            
        return stops.sort(this.Asc(this.sortType));
      }
      else{
        return stops.sort(this.Desc(this.sortType));
      }      
    },
    pageStart(){
      return (this.selectPage - 1) * this.limit; //<= 取代這兒
    },        
  },
  methods: {
    sortStop(type){
      this.sortType = type;
      this.isReverse = !this.isReverse;
    },
    Desc(attr) {
      return function(a,b){
        var val1 = a[attr];
        var val2 = b[attr];
        return val2 - val1;
        }
    },
    Asc(attr) {
      return function(a,b){
        var val1 = a[attr];
        var val2 = b[attr];
        return val1 - val2;
        }
    },    
    timeFormat(t){
      var date = [], time = [];
      date.push(t.substr(0, 4));
      date.push(t.substr(4, 2));
      date.push(t.substr(6, 2));
      time.push(t.substr(8, 2));
      time.push(t.substr(10, 2));
      time.push(t.substr(12, 2));
      return date.join("/") + ' ' + time.join(":");
    },
  }
}
</script>
