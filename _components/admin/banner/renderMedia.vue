<template>
  <div class="q-mt-md">

  </div>
</template>

<script>
  import { eventBus } from 'src/plugins/utils'
  export default {
    props:{
      banner:{
        type: Object,
        default: ()=>({
          imageUrl: '',
        }),
        required: true
      },
      index:{
        type: Number,
        required: true
      }
    },
    methods:{
      deleteBannerDialog(slideId, pos){
        this.$q.dialog({
          title: 'Confirm',
          ok: 'Delete',
          message: 'You are sure to eliminate this slide?',
          cancel: true,
          persistent: true
        }).onOk(() => {
          this.$crud.delete('apiRoutes.qbanner.banners', slideId).then(response => {
            this.$alert.info({ message: this.$tr('isite.cms.message.recordDeleted') })
            //this.position.banners.splice(pos, 1)
            eventBus.emit('deleteBanner', 'deleteBanner')
          }).catch(error => {
            this.$alert.error({ message: this.$tr('isite.cms.message.recordNoDeleted'), pos: 'bottom' })
          })
        }).onCancel(() => {})
      },
      deleteBanner (slideId, pos) {

      },
    }
  }
</script>
