<template>
  <div class="row">
    <div class="col-12" style="width: 100%">
      <q-form
        class="q-mt-lg"
        @submit="updateOrCreateSlider(form)"
        ref="formContent"
        @validation-error="$alert.error($tr('isite.cms.message.formInvalid'))"
        autocomplete="off">
        <q-input
          :label="`${$tr('isite.cms.form.name')} *`"
          type="text"
          outlined
          dense
          :rules="[val => !!val || $tr('isite.cms.message.fieldRequired')]"
          v-model="form.name"/>
        <q-input
          :label="`${$tr('isite.cms.form.slug')} *`"
          type="text"
          outlined
          dense
          :rules="[val => !!val || $tr('isite.cms.message.fieldRequired')]"
          v-model="form.systemName"/>
        <q-select
          :label="$tr('isite.cms.form.status')"
          v-model="form.active"
          emit-value
          map-options
          :options="[
            {label : $tr('isite.cms.label.enabled'), value : true},
            {label : $tr('isite.cms.label.disabled'), value : false},
          ]"
          outlined
          dense/>
        <q-checkbox v-model="form.showAsPopup" :label="$tr('ibanners.cms.label.showAsPopup')"
                    :true-value="true" :false-value="false" />
      </q-form>
      <div class="text-right">
        <q-btn
          @click="$refs.formContent.submit()"
          icon="fas fa-save"
          class="btn-small"
          color="green"
          :label="$tr('isite.cms.label.save')"
          unelevated
          rounded
          :loading="loading"/>
      </div>
    </div>
    <inner-loading :visible="loading"/>
  </div>
</template>

<script>
  export default {
    name: 'positionsForm',
    props: {
      formData: {type: Object}
    },
    watch: {
      formData: {
        deep: true,
        handler: function () {
          this.setFormData()
        }
      }
    },
    mounted() {
      this.$nextTick(function () {
        this.setFormData()
      })
    },
    data() {
      return {
        loading: false,
        form: {options: {}}
      }
    },
    methods: {
      setFormData() {
        this.form = this.$clone(this.formData)
      },
      updateOrCreateSlider(data) {
        this.loading = true
        if (this.form.id) {
          this.$crud.update('apiRoutes.qbanner.positions', data.id, data).then(response => {
            this.$alert.info({message: this.$tr('isite.cms.message.recordUpdated')})
            this.loading = false
          }).catch(error => {
            this.$alert.error({message: this.$tr('isite.cms.message.recordNoUpdated')})
            this.loading = false
          })
        }
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
            create: this.$hasAccess('isite.master.records.create'),
            edit: this.$hasAccess('isite.master.records.edit'),
            index: this.$hasAccess('isite.master.records.index'),
            destroy: this.$hasAccess('isite.master.records.destroy')
          }
        }
        return response
      }
    }
  }
</script>

<style scoped>

</style>
