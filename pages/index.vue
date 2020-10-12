<template>
  <div class="container mt-5">

    <div v-for="joke in jokes" :key="joke.id">
      <p>{{ joke.joke }}</p>
    </div>

    <div class="card" style="width: 18rem;">
      <div class="card-body">
        <h5 class="card-title">Card title</h5>
        <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
      </div>
    </div>

    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li class="page-item">
          <button type="button" class="page-link" v-if="page != 1" @click="page--"> Previous </button>
        </li>
        <li class="page-item">
          <button type="button" class="page-link" v-for="pageNumber in pages.slice(page-1, page+5)" @click="page = pageNumber"> {{pageNumber}} </button>
        </li>
        <li class="page-item">
          <button type="button" @click="page++" v-if="page < pages.length" class="page-link"> Next </button>
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
      page: 1,
      perPage: 9,
      pages: [],
    }
  },

  async created() {
    let url = `http://localhost:8000/api/jokes?page=`;
    const jokes = await axios.get(url);
    this.jokes = jokes.data.results;
    console.log('jokes --> ', this.jokes)
  },

}
</script>
