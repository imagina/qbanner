<template></template>
<script>
  export default {
    data() {
      return {
        crudId: this.$uid()
      }
    },
    computed: {
      crudData() {
        return {
          crudId: this.crudId,
          entityName: config("main.qbanner.entityNames.position"),
          apiRoute: 'apiRoutes.qbanner.positions',
          permission: 'ibanners.positions',
          extraFormFields: 'ibanners.crud-fields.positions',
          create: {
            title: this.$tr('ibanners.cms.newBanner'),
          },
          read: {
            columns: [
              {name: 'id', label: this.$tr('isite.cms.form.id'), field: 'id', style: 'width: 50px'},
              {name: 'name', label: this.$tr('isite.cms.form.name'), field: 'name', align: 'rigth'},
              {name: 'systemName', label: this.$tr('isite.cms.form.systemName'), field: 'systemName', align: 'rigth'},
              {name: 'active', label: this.$tr('isite.cms.form.status'), field: 'active', align: 'left'},
              {
                name: 'created_at', label: this.$tr('isite.cms.form.createdAt'), field: 'createdAt', align: 'left',
                format: val => val ? this.$trd(val) : '-',
              },
              {name: 'actions', label: this.$tr('isite.cms.form.actions'), align: 'left'},
            ],
            requestParams: {}
          },
          update: {
            to: 'qbanner.admin.positions.show'
          },
          delete: true,
          formLeft: {
            id: {value: ''},
            userId: {value: this.$store.state.quserAuth.userId},
            name: {
              value: '',
              type: 'input',
              props: {
                label: `${this.$tr('isite.cms.form.name')}*`,
                rules: [
                  val => !!val || this.$tr('isite.cms.message.fieldRequired')
                ],
              },
            },
            systemName: {
              value: '',
              type: 'input',
              props: {
                label: `${this.$tr('isite.cms.form.systemName')}*`,
                rules: [
                  val => !!val || this.$tr('isite.cms.message.fieldRequired')
                ],
              }
            },
            active: {
              value: '1',
              type: 'select',
              props: {
                label: this.$tr('isite.cms.form.status'),
                options: [
                  {label: this.$tr('isite.cms.label.enabled'), value: '1'},
                  {label: this.$tr('isite.cms.label.disabled'), value: '0'},
                ]
              }
            },
            showAsPopup: {
              value: false,
              type: 'checkbox',
              props: {
                label: this.$tr('ibanners.cms.label.showAsPopup'),
                trueValue: true,
                falseValue: false
              }
            },
          },
          formRight: {},
        }
      },
      //Crud info
      crudInfo() {
        return this.$store.state.qcrudComponent.component[this.crudId] || {}
      }
    },
  }
</script>
