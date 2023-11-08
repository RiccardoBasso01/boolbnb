<script>
import ApartmentCard from './ApartmentCard.vue';
export default {
    props: {
        apartments: Array,
        title: String,
        infoMessage: String,
        cardLoader: Boolean
    },
    components: { ApartmentCard }
}
</script>

<template>
    <div class="container">

        <h5 v-if="title">{{ title }}</h5>

        <div class="d-flex align-items-center gap-1 mb-3">
            <h6>Come ordiniamo i risultati</h6>
            <button class="button-info" type="button" data-bs-toggle="dropdown" aria-expanded="false">
                <font-awesome-icon :icon="['fas', 'question']" size="xs" />
            </button>
            <div class="dropdown-menu">{{ infoMessage }} </div>
        </div>

        <div v-if="apartments.length"
            class="row row-cols-1 row-cols-sm-2 row-cols-lg-3 row-cols-xl-4 row-cols-xxl-5 g-5 g-sm-3">
            <div v-for="apartment in apartments" :key="apartment.id" class="col">
                <!--Card-->
                <ApartmentCard :apartment="apartment" />
            </div>

            <div v-if="cardLoader" v-for="card in 10" class="col">
                <div class="card" aria-hidden="true">
                    <div class="img-card-top card-img placeholder"></div>
                    <div class="card-body">
                        <h5 class="card-title placeholder-glow">
                            <span class="placeholder col-8"></span>
                        </h5>
                        <p class="card-text placeholder-glow">
                            <span class="placeholder col-7"></span>
                            <span class="placeholder col-6"></span>
                        </p>
                    </div>
                </div>
            </div>
        </div>
        <h4 v-else class="text-center mt-5">Nessun risultato</h4>
    </div>
</template>

<style lang="scss" scoped>
@use '../../assets/scss/vars' as *;

.container {
    margin: 20px auto;
}

h6 {
    font-size: 12px;
    color: #717171;
    font-weight: 300;
}

.button-info {
    @include circle(15px);
    @include flex;
    border: 2px solid#717171;
    color: #717171;
}

.dropdown-menu {
    font-size: 14px;
    padding: 8px 10px;
}

// Card loader placeholder 
.card {
    border: none;
    border-radius: 0;
}

.card-img {
    position: relative;
    aspect-ratio: 1;
    overflow: hidden;
}

.placeholder {
    animation: fade 1s linear infinite alternate;
    border-radius: 5px;
}

@keyframes fade {
    from {
        background-color: #878787;
    }

    to {
        background-color: $light-grey;
    }
}

.card-body {
    padding: 16px 0;
}
</style>
