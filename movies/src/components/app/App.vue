<template>
  <div class="app font-monospace">
    <div class="content">
      <AppInfo :allMoviesCount="movies.length" :favouriteMoviesCount="movies.filter(el => el.favourite).length" />
      <div class="search-panel">
        <SearchPanel :onUpdateTermHandler="onUpdateTermHandler" />
        <AppFilter :updateFilterHandler="updateFilterHandler" :filterName="filter" />
      </div>

      <MovieList :movies="onFilterHandler(onSearchHandler(movies, term), filter)" @onToggle="onToggleHandler"
        @onRemove="onRemoveHandler" />

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
      movies: [
        {
          name: "Omar",
          views: 889,
          favourite: false,
          like: true,
          id: 1,
        },
        {
          name: "Empire of Usman",
          views: 777,
          favourite: false,
          like: false,
          id: 2,
        },
        {
          name: "Ertugrul",
          views: 182,
          favourite: true,
          like: false,
          id: 3,
        },
      ],
      term: "",
      filter: "all",
    }
  },
  methods: {
    createMovie(movie) {
      this.movies.push(movie)
    },

    onToggleHandler({ id, prop }) {
      this.movies = this.movies.map(item => {
        if (item.id == id) {
          return { ...item, [prop]: !item[prop] }
        }
        return item
      })
    },

    onRemoveHandler(id) {
      this.movies = this.movies.filter(c => c.id !== id)
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
          return arr.filter(c => c.favourite)
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
    }
  }
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
</style>