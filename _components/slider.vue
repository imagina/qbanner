<template>
  <div id="carousel" v-if="banners && banners.length">
    <q-carousel class="text-white"
                infinite autoplay
                :height="this.$q.platform.is.desktop ? '500px' : '320px'">
      <!--= banners =-->
      <q-carousel-banner :img-src="banner.imageUrl"
                        v-for="(banner,index) in banners" :key="index">
        <div class="filter-img" style="background-image: url('statics/filter-carousel.png')">
          <div class="q-container row items-center full-height">
            <div class="col-1"></div>
            <div class="col-11 col-md-6">
              <h3 class="q-ma-none q-mb-md">
                {{banner.title}}
              </h3>
              <span>
                {{banner.caption}}
              </span>
            </div>
          </div>
        </div>
      </q-carousel-banner>

      <!--= Controls =-->
      <q-carousel-control slot="control-nav"
                          class="q-container"
                          slot-scope="carousel"
                          position="center">
        <q-btn
          @click="carousel.previous"
          icon="fas fa-arrow-left"
          class="btnLeft" round flat
        />
        <q-btn
          @click="carousel.next"
          icon="fas fa-arrow-right"
          class="btnRight" flat round
        />
      </q-carousel-control>
    </q-carousel>
  </div>
</template>
<script>
  /*Services*/
  import sliderServices from 'modules/qbanner/_services/slider'
  export default {
    props: {
      id : {default:1}
    },
    components: {},
    watch: {},
    mounted() {
      this.$nextTick(function () {
        this.getData()
      })
    },
    data() {
      return {
        slides: []
      }
    },
    methods: {
      getData(){
        sliderServices.show(this.id).then((response) => {
          this.slides = response.data
        })
      }
    }
  }
</script>
<style lang="scss">
  #carousel
    .q-carousel-slide
      padding 0
      .filter-img
        height 100%
        width 100%
    .q-carousel-control
      margin 0 !important
      padding 0 !important
      .q-btn
        &.btnLeft
          margin-left -34px
        &.btnRight
          float right
          margin-right -34px
</style>
