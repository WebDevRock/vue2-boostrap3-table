<template>
    <section>
        <table class="table table-bordered">
    <thead>
      <tr>
        <th @click="sort('name')">Name</th>
        <th @click="sort('age')">Age</th>
        <th @click="sort('breed')">Breed</th>
        <th @click="sort('gender')">Gender</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="cat in sortedCats" :key="cat.name">
        <td>{{cat.name}}</td>
        <td>{{cat.age}}</td>
        <td>{{cat.breed}}</td>
        <td>{{cat.gender}}</td>
      </tr>
    </tbody>
  </table>
  <nav aria-label="...">
      <ul class="pagination">
          <li :class="currentPage==1?'disabled':''"><a href="#" aria-label="Previous" @click="prevPage"><span aria-hidden="true">«</span></a></li>
          <li class="active"><a href="#">1 <span class="sr-only">(current)</span></a></li>
          <li><a href="#">2</a></li>
          <li><a href="#">3</a></li>
          <li><a href="#">4</a></li>
          <li><a href="#">5</a></li>
          <li :class="currentPage==1?'disabled':''"><a href="#" aria-label="Next" @click="nextPage"><span aria-hidden="true">»</span></a></li>
        </ul>
    </nav>
  <p>
  <button @click="prevPage">Previous</button>
  <button @click="nextPage">Next</button>
  </p>

  debug: sort={{currentSort}}, dir={{currentSortDir}}, page={{currentPage}}
    </section>
</template>

<script>
export default {
    name: "Bootsrap3Table",
    data(){
        return {
            cats:[],
            currentSort:'name',
            currentSortDir:'asc',
            pageSize:5,
            currentPage:1
        }

  },
    created:function() {
        fetch('https://api.myjson.com/bins/s9lux')
        .then(res => res.json())
        .then(res => {
        this.cats = res;
        })
  },
  methods:{
    sort:function(s) {
      //if s == current sort, reverse
      if(s === this.currentSort) {
        this.currentSortDir = this.currentSortDir==='asc'?'desc':'asc';
      }
      this.currentSort = s;
    },
    nextPage:function() {
      if((this.currentPage*this.pageSize) < this.cats.length) this.currentPage++;
    },
    prevPage:function() {
      if(this.currentPage > 1) this.currentPage--;
    }

  },
  computed:{
    sortedCats:function() {
      return this.cats.sort((a,b) => {
        let modifier = 1;
        if(this.currentSortDir === 'desc') modifier = -1;
        if(a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
        if(a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
        return 0;
      }).filter((row, index) => {
        let start = (this.currentPage-1)*this.pageSize;
        let end = this.currentPage*this.pageSize;
        if(index >= start && index < end) return true;
      });
    }
  }
}
</script>

<style>

</style>
