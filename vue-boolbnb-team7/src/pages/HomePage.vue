<script>
import AppNavbar from '../components/AppNavbar.vue';
import AppLoader from '../components/AppLoader.vue';
import ApartmentsList from '../components/apartment/ApartmentsList.vue';

import axios from 'axios';
const baseUri = 'http://127.0.0.1:8000/api/apartments';

export default {
    components: { AppNavbar, ApartmentsList, AppLoader },
    data: () => ({
        apartmentsPromoted: [],
        apartmentsRandom: [],
        isLoading: true,
        currentPage: 1,
        lastPage: null,
        cardLoader: false,
        rand_seed: null,
    }),
    methods: {
        fetchApartments(endpoint = '', successCallback) {
            axios.get(baseUri + endpoint)
                .then(successCallback)
                .catch(err => {
                    console.error(err);
                })
                .then(() => {
                    this.isLoading = false;
                });
        },
        infiniteScroll() {
            if ((this.currentPage >= this.lastPage || this.cardLoader)) return;

            // Height of apartment list (section that you see on the page)
            const apartmentListHeight = this.$refs.apartmentList.getBoundingClientRect().height;
            // The pixels that scrolled the apartment list from the top side of the window
            const apartemntListTopScroll = this.$refs.apartmentList.scrollTop;
            // Total height of the entire element, visible and invisible
            const totalHeight = this.$refs.apartmentList.scrollHeight;
            // How many pixels before the bottom does the call start
            const tollerance = 10;

            if ((apartemntListTopScroll + apartmentListHeight) + tollerance >= totalHeight) {
                this.currentPage++

                if (!this.cardLoader) {
                    window.requestAnimationFrame(() => {
                        this.cardLoader = true;

                        // Call api for another 10 apartments
                        this.fetchApartments(`/random?page=${this.currentPage}&rand_seed=${this.rand_seed}`, res => { this.apartmentsRandom.push(...res.data.data) });

                        setTimeout(() => {
                            this.cardLoader = false;
                        }, 1000)
                    });
                }
            }
        }

    },
    created() {
        // Set random seed (to avoid duplication)
        this.rand_seed = Math.random();

        // Fetch Promoted List
        this.fetchApartments('/promoted', res => { this.apartmentsPromoted = res.data; });

        // Fetch Random List
        this.fetchApartments(`/random?page=${this.currentPage}&rand_seed=${this.rand_seed}`, res => { this.apartmentsRandom = res.data.data; this.lastPage = res.data.last_page });
    }
}
</script>

<template>
    <main @scroll="infiniteScroll" ref="apartmentList">

        <!-- Promoted Appartments -->
        <ApartmentsList v-if="!isLoading && apartmentsPromoted.length" :apartments="apartmentsPromoted"
            infoMessage="I boolbnb in evidenza sono consigliati dal nostro team!" title="Boolbnb in evidenza" />

        <!-- Random Appartments -->
        <ApartmentsList v-if="!isLoading" :apartments="apartmentsRandom" title="I nostri boolbnb"
            infoMessage="I boolbnb vengono mostrati in ordine casuale" :cardLoader="cardLoader" />

        <!-- Loader -->
        <AppLoader :is-loading="isLoading" :cardLoading="true" />
    </main>
</template>

<style>
main {
    position: relative;
}
</style>