<template>
  <div class="row relative-position">
    <div class="col-12">
      <!---Component CRUD Slided -->
      <div class="text-right q-mb-md">
        <crud :crud-data="import('@imagina/qbanner/_crud/banners')" ref="crudBanner"
              type="button-create" :crud-props="{unelevated : true, rounded : true}"
              @updated="$emit('refresh')" @created="$emit('refresh')"
              :custom-data="{formLeft : {positionId: {value : $route.params.id}}}"/>
      </div>
      <!--Items-->
      <draggable @change="updateOrderBanners" v-model="position.banners" group="banners">
        <!--Item-->
        <q-card class="my-card q-mb-md" v-for="(banner,index) in position.banners" :key="'banner'+index">
          <div class="row items-center q-pa-sm">
            <!--Title-->
            <div class="col-6">
              <q-chip square icon="fas fa-images" text-color="white" :color="banner.active ? 'green' : 'grey'">
                {{ banner.title }}
              </q-chip>
            </div>
            <!--Actions-->
            <div class="col-6 text-right">
              <q-btn color="blue-4" size="sm" round icon="fas fa-pen" unelevated class="q-mr-sm"
                     @click="$refs.crudBanner.update(banner)"/>
              <q-btn @click="deleteBannerDialog(banner.id, index)" color="red" unelevated
                     size="sm" round icon="far fa-trash-alt"/>
            </div>
          </div>
          <q-separator/>
          <div class="full-width" v-if="banner.imageUrl">
            <iframe
                v-if="~banner.imageUrl.indexOf('youtube.com')"
                width="100%"
                height="300"
                :src="banner.imageUrl"
                frameborder="0"
                allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                allowfullscreen>
            </iframe>
            <video
                width="100%"
                height="300"
                v-else-if="~banner.imageUrl.indexOf('.mp4')"
                class='img-responsive center-block'
                loop
                controls='false'>
              <source :src="banner.imageUrl" type='video/mp4'>
            </video>
            <div
                v-else
                :style="`
        background: url('${banner.imageUrl}');
        background-size: cover;
        background-position: center;
        height: 300px;
        display: block;
        max-width: 100%;`">
            </div>
          </div>
          <div class="full-width" v-else-if="banner.url">
            <iframe
                v-if="~banner.url.indexOf('youtube.com')"
                width="100%"
                height="300"
                :src="banner.url"
                frameborder="0"
                allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                allowfullscreen>
            </iframe>
            <video
                width="100%"
                height="300"
                v-else-if="~banner.url.indexOf('.mp4')"
                class='img-responsive center-block'
                loop
                controls='false'>
              <source :src="banner.url" type='video/mp4'>
            </video>
          </div>
        </q-card>
      </draggable>
    </div>
    <!--Loading-->
    <inner-loading :visible="loading"/>
  </div>
</template>

<script>
import renderMedia from '@imagina/qbanner/_components/admin/banner/renderMedia'
import draggable from 'vuedraggable'

export default {
  name: 'positionBanners',
  components: {
    draggable,
    renderMedia
  },
  mounted() {
    this.init()
  },
  data() {
    return {
      loading: false
    }
  },
  props: {
    position: {
      type: Object,
      default: () => ({
        id: 0,
      })
    },
  },
  watch: {},
  methods: {
    init() {
      this.openEditSlide()
    },
    openEditSlide() {
      setTimeout(() => {
        if (this.$route.query.edit) {
          let slideToEdit = (this.position.banners || []).find(item => item.id == this.$route.query.edit)
          if (slideToEdit) this.$refs.crudBanner.update(slideToEdit)
        }
      }, 500)
    },
    hasPermissionRecordMAster(record) {
      let options = record.options || false
      let response = {
        create: true,
        edit: true,
        index: true,
        destroy: true,
      }
      if (options && parseInt(options.masterRecord)) {
        response = {
          create: this.$auth.hasAccess('isite.master.records.create'),
          edit: this.$auth.hasAccess('isite.master.records.edit'),
          index: this.$auth.hasAccess('isite.master.records.index'),
          destroy: this.$auth.hasAccess('isite.master.records.destroy')
        }
      }
      return response
    },
    log() {
      let banners = this.position.banners.map(banner => ({id: banner.id}))
      console.error(banners)
    },
    updateOrderBanners() {
      let banners = this.position.banners.map(banner => ({id: banner.id}))
      let data = {
        banners: banners
      }
      this.loading = true
      this.$crud.create('apiRoutes.qbanner.orderBanners', data).then(response => {
        this.$alert.success({message: `${this.$tr('isite.cms.message.recordUpdated')}`})
        this.loading = false
      }).catch(error => {
        this.$alert.error({message: this.$tr('isite.cms.message.errorRequest'), pos: 'bottom'})
        console.warn(error)
        this.loading = false
      })
    },
    deleteBannerDialog(bannerId, pos) {
      this.$q.dialog({
        title: 'Confirm',
        ok: 'Delete',
        message: 'You are sure to eliminate this banner?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.$crud.delete('apiRoutes.qbanner.banners', bannerId).then(response => {
          this.$alert.info({message: this.$tr('isite.cms.message.recordDeleted')})
          //this.position.banners.splice(pos, 1)
          this.$root.$emit('deleteBanner', 'deleteBanner')
        }).catch(error => {
          this.$alert.error({message: this.$tr('isite.cms.message.recordNoDeleted'), pos: 'bottom'})
        })
      }).onCancel(() => {
      })
    },
  }
}
</script>

<style scoped>

</style>
