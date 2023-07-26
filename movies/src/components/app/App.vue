<template>
  <div class="app font-monospace">
    <div class="content">
      <AppInfo :allMoviesCount="movies.length" :favouriteMoviesCount="movies.filter(el => el.favourite).length" />
      <div class="search-panel">
        <SearchPanel :onUpdateTermHandler="onUpdateTermHandler" />
        <AppFilter :updateFilterHandler="updateFilterHandler" :filterName="filter" />
      </div>
      <Box v-if="isLoading">
        <Loader />
      </Box>
      <Box v-else-if="!movies.length">
        <h3 class="text-center font-bold text-danger">Kinolar yo'q:(</h3>
      </Box>
      <MovieList v-else :movies="onFilterHandler(onSearchHandler(movies, term), filter)" @onToggle="onToggleHandler"
        @onRemove="onRemoveHandler" />

      <Box v-if="movies.length > 0">
        <nav aria-label="Page navigation example">
          <ul class="pagination justify-content-center">

            <li class="page-item" style="cursor: pointer;" v-if="1 !== page" @click="previousPageHandler(page)">
              <a class="page-link">Previous</a>
            </li>

            <li v-for="pageNumber in totalPages" :key="pageNumber" @click="changePageHandler(pageNumber)">

              <span style="cursor: pointer;" class="page-link" :class="{ 'active': pageNumber == page }">{{
                pageNumber }}</span>
            </li>

            <li class="page-item" style="cursor: pointer;" v-if="totalPages !== page" @click="nextPageHandler(page)">
              <span class="page-link">Next</span>
            </li>
          </ul>
        </nav>

      </Box>
      <!-- <div v-if="filter == 'all'">Filter All</div> -->
      <!-- <MovieAddForm  @createMovie="createMovie" :class="[filter == 'popular' && 'none']" /> -->
      <MovieAddForm @createMovie="createMovie" />

    </div>
  </div>
</template>

<script>
import AppInfo from "@/components/app-info/AppInfo.vue"
import SearchPanel from "@/components/search-panel/SearchPanel.vue"
import AppFilter from "@/components/app-filter/AppFilter.vue"
import MovieList from "@/components/movie-list/MovieList.vue"
import MovieAddForm from "@/components/movie-add-form/MovieAddForm.vue"
import axios from 'axios'


export default {
  components: {
    AppInfo,
    SearchPanel,
    AppFilter,
    MovieList,
    MovieAddForm
  },
  data() {
    return {
      movies: [],
      term: "",
      filter: "all",
      isLoading: false,
      limit: 10,
      page: 1,
      totalPages: 0,
    }
  },
  methods: {
    async createMovie(movie) {
      try {
        const response = await axios.post("https://jsonplaceholder.typicode.com/posts", movie)
        this.movies.push(response.data)
      } catch (error) {
        alert(error.message)
      }
    },

    onToggleHandler({ id, prop }) {
      this.movies = this.movies.map(item => {
        if (item.id == id) {
          return { ...item, [prop]: !item[prop] }
        }
        return item
      })
    },

    async onRemoveHandler(id) {
      try {
        const response = await axios.delete(`https://jsonplaceholder.typicode.com/posts/${id}`)
        console.log(response);
        this.movies = this.movies.filter(c => c.id !== id)
      } catch (error) {
        console.log(error.message);
      }
    },

    onSearchHandler(arr, term) {
      if (term.length == 0) {
        return arr
      }

      return arr.filter(c => c.name.toLowerCase().indexOf(term) > -1)
    },

    onFilterHandler(arr, filter) {
      switch (filter) {
        case "popular":
          return arr.filter(c => c.favourite || c.like)
        case "mostViews":
          return arr.filter(c => c.views > 500)
        default:
          return arr
      }
    },

    onUpdateTermHandler(term) {
      this.term = term
    },

    updateFilterHandler(filter) {
      this.filter = filter
    },



    // mountedLog() {
    //   console.log('Mounted!');
    // },

    // updateLog() {
    //   console.log('Updated!');
    // },

    async fetchMovie() {
      this.isLoading = true
      try {
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _limit: this.limit,
            _page: this.page,
          }
        })
        const newArr = response.data.map(item => ({
          id: item.id,
          name: item.title,
          like: false,
          favourite: false,
          views: (item.id + 50) * Math.round(Math.random() * 10)
        }))
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.movies = newArr
      } catch (error) {
        alert(error.message);
      }
      finally {
        this.isLoading = false
      }
    },

    changePageHandler(page) {
      this.page = page
      this.fetchMovie()
    },

    previousPageHandler(page) {
      if (this.page > 1) {
        this.page = page - 1
        this.fetchMovie()
      }
    },

    nextPageHandler(page) {
      if (this.page < this.totalPages) {
        this.page = page + 1
        this.fetchMovie()
      }
    },
  },

  mounted() {
    this.fetchMovie()
  },

  // watch - razvedkachi
  // watch: {
  //   page() {
  //     this.fetchMovie()
  //   }
  // }

  // mounted() {
  //   this.mountedLog()
  // },
  // updated() {
  //   this.updateLog()
  // },
}
</script>

<style>
.app {
  height: 100vh;
  color: black;
}

.content {
  width: 1000px;
  min-height: 700px;
  background: #fff;
  margin: 0 auto;
  padding: 5rem 0;
}

.search-panel {
  margin-top: 2rem;
  padding: 1.5rem;
  background: #fcfaf5;
  border-radius: 4px;
  box-shadow: 15px 15px 15px rgba(0, 0, 0, .15);
}

.none {
  display: none;
}
</style>