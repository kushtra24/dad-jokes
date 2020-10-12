<template>
  <div class="container mt-5">

    <div class="d-flex justify-content-center" v-if="loading">
      <div class="spinner-border" role="status">
        <span class="sr-only">Loading...</span>
      </div>
    </div>

    <div class="jokes" v-show="!loading">

      <div class="row">
        <div class="col-lg-6 mb-2">
        <form @submit.prevent="getJokes">
            <div class="form-group row">
              <input type="text" class="form-control col-10" v-model="term" placeholder="Search Jokes">
              <button type="submit" class="btn btn-primary col-2">Search</button>
            </div>
          </form>
        </div>
        <div class="col-lg-6 p-0 mb-2">
          <div class="btn-group float-right" >
            <button type="button" class="btn btn-danger dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
              Limit
            </button>
            <div class="dropdown-menu">
              <a class="dropdown-item" @click="changeLimit(5)" href="#">5</a>
              <a class="dropdown-item" @click="changeLimit(10)" href="#">10</a>
              <a class="dropdown-item" @click="changeLimit(20)" href="#">20</a>
            </div>
          </div>
        </div>
      </div>

     <div class="row">
       <div class="alert alert-danger text-center" v-if="nothing" role="alert">
         There are no Jokes with these conditions
       </div>
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
  </div>
</template>

<script>
  import axios from "axios";

  export default {

  data() {
    return {
      jokes: [],
      currentPage: 1,
      limit: 20,
      totalPages: 20,
      pages: [],
      term: '',
      loading: false,
      nothing: false
    }
  },

    methods:{
     async getJokes() {
        this.loading = true;

        let url = 'http://localhost:8000/api/jokes?current_page=' + this.currentPage;

        if (this.term) {
          url += '&term=' + this.term
        }

        if (this.limit) {
          url += '&limit=' + this.limit
        }

        const jokes = await axios.get(url);
        this.jokes = jokes.data;
        this.totalPages = jokes.data.total_pages;
        this.currentPage = jokes.data.current_page;
        this.limit = jokes.data.limit;
        this.loading = false;
        if (this.jokes.results.length < 1) {
          this.nothing = true;
        }
        this.setPages();
      },

      changeLimit(value) {
       this.limit = value;
       this.getJokes();
      },

      nextPage() {
        this.currentPage++;
        this.getJokes()
      },

      prevPage() {
       this.currentPage--;
       this.getJokes();
      },

      setPage(pageNumber) {
        this.currentPage = pageNumber;
        this.getJokes();
      },

      setPages() {
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
