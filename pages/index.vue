<template>
  <div class="container mt-5">

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
          <button type="button" class="page-link" v-if="currentPage != 1" @click="prevPage()"> Previous </button>
        </li>
        <li class="page-item">
          <button type="button" class="page-link" v-for="pageNumber in pages.slice(page-1, page+5)" @click="page = pageNumber"> {{pageNumber}} </button>
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
      totalPages: 20
    }
  },
    
    methods:{
     async getJokes() {
        let url = `http://localhost:8000/api/jokes?current_page=` + this.currentPage;
        const jokes = await axios.get(url);
        this.jokes = jokes.data;
        this.totalPages = jokes.data.total_pages;
        this.currentPage = jokes.data.current_page;
        this.currentPage = jokes.data.current_page;
      },
      nextPage() {
        this.currentPage++;
        this.getJokes()
      },
      prevPage() {
       this.currentPage--;
       this.getJokes();
      }
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
