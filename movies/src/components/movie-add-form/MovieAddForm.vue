<template>
    <div class="movie-add-form">
        <h3>
            Yangi kino qoshish
        </h3>
        <form class="add-form d-flex" @submit.prevent>
            <input type="text" class="form-control new-movie-label" placeholder="Qanday kino?" :value="name"
                @:input="name = $event.target.value">
            <input type="number" class="form-control new-movie-label" placeholder="Nechi marotaba ko'rilgan?" :value="views"
                @:input="views = $event.target.value">
            <button type="submit" class="btn btn-outline-dark" @click="addMovie">Qo'shish</button>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            name: "",
            views: ""
        }
    },
    methods: {
        addMovie() {
            if (!this.name || !this.views) return
            const newMovie = {
                name: this.name,
                views: this.views,
                favourite: false,
                like: false,
                id: Date.now()
            }
            this.$emit("createMovie", newMovie)
            this.name = ""
            this.views = ""
        }
    },
}
</script>

<style scoped>
.movie-add-form {
    margin-top: 2rem;
    padding: 1.5rem;
    background: #fcfaf5;
    border-radius: 4px;
    box-shadow: 15px 15px 15px rgba(0, 0, 0, .15);
}
</style>