<template>
  <div
    id="pageId"
    class="q-layout-page layout-padding">
    <div class="q-mb-lg backend-page">
      <div class="row">
        <div class="col-12 flex justify-end q-my-sm">
          <q-breadcrumbs>
            <q-breadcrumbs-el label="Banners" :to="{name: 'qbanner.admin.positions'}"/>
            <q-breadcrumbs-el label="Banners" />
          </q-breadcrumbs>
        </div>
      </div>
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
                <positionBanners :position="positionData" />
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
    name: 'positionShow',
    components:{
      positionForm,
      positionBanners
    },
    data () {
      return {
        loading: false,
        positionData:{}
      }
    },
    beforeUnmount () {
      eventBus.off('deleteBanner', this.getPosition)
    },
    created() {
      this.getPosition(true)
      eventBus.on('deleteBanner', this.getPosition)
    },
    methods:{
      getPosition( refresh = false ){
        let criteria = this.$route.params.id
        let params = {
          refresh,
          params: {}
        }
        this.loading = true
        this.$crud.show('apiRoutes.qbanner.positions', criteria, params).then( response => {
          this.positionData =  response.data
          this.position.options = response.data.options || {showAsPopup: '0', masterRecord: '0'}
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
