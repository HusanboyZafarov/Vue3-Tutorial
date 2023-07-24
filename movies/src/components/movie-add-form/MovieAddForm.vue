<template>
    <Box>
        <h3>
            Yangi kino qoshish
        </h3>
        <form class="add-form d-flex" @submit.prevent>
            <Input class="new-movie-label" v-model="name" placeholder="Qanday kino?" />
            <Input type="number" class="new-movie-label" v-model="views" placeholder="Nechi marotaba ko'rilgan?" />

            <PrimaryButton type="submit" class="btn-outline-dark" @click="addMovie">Qo'shish</PrimaryButton>
        </form>
    </Box>
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

<style scoped></style>