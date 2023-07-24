<template>
    <div class="btn-group ">
        <PrimaryButton v-for="btn in filterButton" :key="btn.name"
            :class="[filterName == btn.name ? 'btn-dark' : 'btn-outline-dark']" @click="filterHandler(btn.name)">
            {{ btn.title }}
        </PrimaryButton>

        <!-- Bu yerda  -->

        <!-- <button class="btn" type="button" @click="filterHandler('all')"
            :class="[filterName == 'all' ? 'btn-dark' : 'btn-outline-dark']">Barcha kinolar</button>
        <button class="btn" type="button" @click="filterHandler('popular')"
            :class="[filterName == 'popular' ? 'btn-dark' : 'btn-outline-dark']"> Mashxur kinolar </button>
        <button button class="btn" type="button" @click="filterHandler('mostViews')"
            :class="[filterName == 'mostViews' ? 'btn-dark' : 'btn-outline-dark']">Eng ko'p ko'rilgan
            kinolar</button> -->
    </div>
</template>

<script>
import PrimaryButton from '@/ui-components/PrimaryButton.vue'
export default {
    props: {
        updateFilterHandler: {
            type: Function,
            required: true,
        },
        filterName: {
            type: String,
            required: true
        }
    },
    data() {
        return {
            filterButton: [
                {
                    title: "Barcha kinolar",
                    name: "all"
                }, {
                    title: "Mashxur kinolar",
                    name: "popular"
                }, {
                    title: "Eng ko'p ko'rilgan kinolar",
                    name: "mostViews"
                },
            ],
            filter: "all",
        }
    },
    methods: {
        filterHandler(filter) {
            this.filter = filter
            this.updateFilterHandler(this.filter)
        }
    },
}
</script>

<style ></style>