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
          entityName: config("main.qbanner.entityNames.banner"),
          apiRoute: 'apiRoutes.qbanner.banners',
          permission: 'ibanners.banners',
          extraFormFields: 'ibanners.crud-fields.banners',
          create: {
            title: this.$tr('ibanners.cms.newBannerItem'),
          },
          read: {
            columns: [],
            requestParams: {}
          },
          update: {
            title: this.$tr('ibanners.cms.updateSlide'),
          },
          delete: true,
          formLeft: {
            id: {value: ''},
            userId: {value: this.$store.state.quserAuth.userId},
            title: {
              value: '',
              type: 'input',
              isTranslatable : true,
              props: {
                label: `${this.$tr('isite.cms.form.title')}*`,
                rules: [
                  val => !!val || this.$tr('isite.cms.message.fieldRequired')
                ],
              },
            },
            uri: {
              value: '',
              type: 'input',
              isTranslatable : true,
              props: {
                label: 'URI',
              },
            },
            url: {
              value: '',
              type: 'input',
              isTranslatable : true,
              props: {
                label: 'URL',
              },
            },
            externalImageUrl: {
              value: '',
              type: 'input',
              isTranslatable : true,
              props: {
                label: `${this.$tr('isite.cms.form.image')}`,
              },
            },
            customHtml: {
              value: '',
              type: 'html',
              isTranslatable: true,
              props: {
                label: `${this.$tr('isite.cms.form.description')}`,
              }
            },
          },
          formRight: {
            active: {
              value: 1,
              type: 'select',
              isTranslatable : true,
              props: {
                label: this.$tr('isite.cms.form.status'),
                options: [
                  {label: this.$tr('isite.cms.label.enabled'), value: 1},
                  {label: this.$tr('isite.cms.label.disabled'), value: 0},
                ]
              }
            },
            targetField: {
              name : 'target',
              value: null,
              type: 'select',
              props: {
                label: this.$tr('isite.cms.form.option'),
                options: [
                  {label: 'Same tab', value: '_self'},
                  {label: 'New tab', value: '_blank'}
                ]
              }
            },
            typeField: {
              name : 'type',
              value: null,
              type: 'select',
              props: {
                label: this.$tr('isite.cms.form.type'),
                options: [
                  {label: 'Auto', value: 'auto'},
                  {label: '360', value: '360'},
                  {label: 'Video', value: 'video'},
                  {label: 'Image', value: 'image'}
                ]
              }
            },
            codeAds: {
              value: '',
              type: 'input',
              isFakeField: true,
              props: {
                label: `${this.$tr('ibanners.cms.label.codeAds')}`,
                type: 'textarea',
                rows: "3"
              },
            },
            mediasSingle: {
              value: {},
              type: 'media',
              props: {
                label: this.$tr('isite.cms.form.image'),
                zone: 'bannerimage',
                entity: "Modules\\Ibanners\\Entities\\Banner",
                entityId: null
              }
            }
          },
        }
      },
      //Crud info
      crudInfo() {
        return this.$store.state.qcrudComponent.component[this.crudId] || {}
      }
    },
  }
</script>
