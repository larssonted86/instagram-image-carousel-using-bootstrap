<template>
<div class="container-fluid p-5"> <!-- start of the page container -->
  <div class="row"> <!-- start of title row -->
    <div class="col">
      <h1 class="display-1 text-white">Carousel widget</h1>
    </div>
  </div> <!-- end of title row -->
  <div class="row widget-row"> <!-- start of widget row -->
    <div class="col">
      <boot-carousel />
    </div>
  </div> <!-- end of widget row -->
</div> <!-- end of page container -->
</template>

<script>
import BootCarousel from "./components/BootCarousel.vue";

export default {
  name: "App",
  components: {
    BootCarousel,
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}


//adding background gradient
.container-fluid {
  margin-left:0;
  margin-right:0;
  height: 100vh;
  background-image: linear-gradient( 135deg, #FFD26F 10%, #3677FF 100%);
}

// adding the glass effect to the row of the widget. this can be altered to fit any style of the rest of the webpage
.widget-row{
  background:rgba(255,255,255,0.15);

box-shadow:0 8px 32px 0 rgba(31,38,135,.37);

backdrop-filter:blur(4.5px);

border-radius:10px;


}
</style>
