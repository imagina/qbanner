<template>
  <div
    id="pageId"
    class="q-layout-page layout-padding">
    <div class="q-mb-lg backend-page">
      <div class="row q-col-gutter-md">
        <div class="col-xs-12 col-md-5">
          <div class="box">
            <div class="row gutter-y-sm">
              <div class="col-12 relative-position">
                <positionForm :form-data="position"/>
                <inner-loading :visible="loading"/>
              </div>
            </div>
          </div>
        </div>
        <div class="col-xs-12 col-md-7">
          <div class="box">
            <div class="row gutter-y-sm">
              <div class="col-12 relative-position">
                <positionBanners :position="position" v-if="!loading" @refresh="getPosition(true)"/>
                <inner-loading :visible="loading"/>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import positionForm from 'modules/qbanner/_components/admin/position/form'
  import positionBanners from 'modules/qbanner/_components/admin/position/banners'
  import { eventBus } from 'src/plugins/utils'

  export default {
    components:{
      positionForm,
      positionBanners
    },
    beforeUnmount () {
      eventBus.off('deleteBanner', this.getPosition)
      eventBus.off('page.data.refresh')
    },
    mounted() {
      this.init()
    },
    data () {
      return {
        loading: false,
        position:{}
      }
    },
    methods:{
      init(){
        eventBus.on('page.data.refresh', () => this.getPosition(true))//Listen refresh event
        this.getPosition(true)
        eventBus.on('deleteSlide', this.getPosition)
      },
      //Get position data
      getPosition( refresh = false ){
        let criteria = this.$route.params.id
        let params = {
          refresh : refresh,
          params: {}
        }
        this.loading = true
        this.$crud.show('apiRoutes.qbanner.positions', criteria, params).then( response => {
          this.position =  response.data
          this.loading = false
        }).catch( error => {
          this.$apiResponse.handleError(error, () => {
            console.warn(error)
            this.loading = false
            this.$alert.error({message: this.$tr('isite.cms.message.errorRequest'), pos: 'bottom'})
          })
        })
      },
    }
  }
</script>
