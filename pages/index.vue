<template>
  <div class="container mt-5" v-if="!loading">

 <div class="row">
   <div class="card col-md-4 col-sm-12" style="width: 18rem;" v-for="joke in jokes.results" :key="joke.id">
     <div class="card-body">
       <h5 class="card-title">{{ joke.id }}</h5>
       <p class="card-text">{{ joke.joke }}</p>
     </div>
   </div>
 </div>

    <nav aria-label="Page navigation" class="marginTop">
      <ul class="pagination">
        <li class="page-item">
          <button type="button" class="page-link" v-if="currentPage !== 1" @click="prevPage()"> Previous </button>
        </li>
        <li class="page-item">
          <button type="button" class="page-link" v-for="pageNumber in pages.slice(currentPage-1, currentPage+5)" @click="setPage(pageNumber)"> {{pageNumber}} </button>
        </li>
        <li class="page-item">
          <button type="button" @click="nextPage()" v-if="currentPage < totalPages" class="page-link"> Next </button>
        </li>
      </ul>
    </nav>

  </div>
</template>

<script>
  import axios from "axios";

  export default {

  data() {
    return {
      jokes: [],
      currentPage: 1,
      perPage: 20,
      totalPages: 20,
      pages: [],
      loading: false
    }
  },

    methods:{
     async getJokes() {
        // this.loading = true;
        let url = `http://localhost:8000/api/jokes?current_page=` + this.currentPage;
        const jokes = await axios.get(url);
        this.jokes = jokes.data;
        this.totalPages = jokes.data.total_pages;
        this.currentPage = jokes.data.current_page;
        this.currentPage = jokes.data.current_page;
        this.setPages();
      },
      nextPage() {
        this.currentPage++;
        this.getJokes()
      },
      prevPage() {
       this.currentPage--;
       this.getJokes();
      },
      // setPage(pageNumber) {
      //   this.currentPage = pageNumber;
      //   this.getJokes();
      // },
      setPages () {
        for (let index = 1; index <= this.totalPages; index++) {
          this.pages.push(index);
        }
      },
    },
    created(){
      this.getJokes();
    },
}
</script>

<style>
  button.page-link {
    display: inline-block;
  }
  button.page-link {
    font-size: 20px;
    color: #29b3ed;
    font-weight: 500;
  }
  .marginTop {
    margin-top: 50px;
  }
</style>
