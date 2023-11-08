<script>
export default {
  props: {
    isLoading: Boolean,
    cardLoading: Boolean
  },
};
</script>
<!-- Cards loader -->
<template>
  <div class="container" v-if="isLoading && cardLoading">
    <div class="row mb-3">
      <h2 class="placeholder-glow mb-2"><span class="placeholder col-5 col-md-4 col-lg-3"></span></h2>
      <h5 class="placeholder-glow"><span class="placeholder col-4 col-md-3 col-lg-2"></span></h5>
    </div>

    <div class="row row-cols-1 row-cols-sm-2 row-cols-lg-3 row-cols-xl-4 row-cols-xxl-5 g-5 g-sm-3">
      <div v-for="card in 10" class="col">
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
  </div>

  <!-- Generic loader -->
  <div class="loader" v-else-if="isLoading">
    <div class="spinner-border" role="status">
      <span class="visually-hidden">Caricamento...</span>
    </div>
  </div>
</template>
  
<style lang="scss" scoped>
@use '../assets/scss/vars' as *;

// Generic loader
.loader {
  position: fixed;
  top: 80px;
  left: 0;
  width: 100%;
  height: calc(100vh - 120px);
  background: $light-grey;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-size: 24px;
}

.spinner-border {
  @include circle(100px);
}

// Cards loader
.container {
  position: absolute;
  inset: 0;
  padding-top: 20px;
}

.card {
  border: none;
  border-radius: 0;
}

.card-img {
  aspect-ratio: 1;
  overflow: hidden;
}

.card-body {
  padding: 16px 0;
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
</style>