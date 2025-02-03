<template>
    <q-dialog
      :model-value="modelValue"
      @update:model-value="$emit('update:modelValue', $event)"
    >
        <q-card>
          <q-card-section>
            <div class="text-h6">TTF Modal No Layout</div>
          </q-card-section>
  
          <q-card-section>
            Modal stuff
            <div
              id="TTF_WIDGET_CONTAINER"
              data-product-id="TH134"
              data-api-key="i8ZWxd3vHEgf8vczXE5N"
              style="border: solid 1px red;"
            ></div>
          </q-card-section>
  
          <q-card-actions align="right">
            <q-btn flat label="Close" color="primary" v-close-popup />
          </q-card-actions>
        </q-card>
    </q-dialog>
  </template>
  
<script>
export default {
  name: 'TrueToFormDialog',
  props: {
    modelValue: {
      type: Boolean,
      required: true
    }
  },
  emits: ['update:modelValue'],
  mounted() {
        this.$nextTick(() => {
          this.LoadWidgetScript()
        })
    
  },
  methods: {
    UnloadWidgetScript () {
      const script = document.querySelector('script[src="https://ttf-widget.pages.dev/assets/integrations/custom.js"]')
      if (script) {
        document.body.removeChild(script)
      }
    },
    LoadWidgetScript () {
      const script = document.querySelector('script[src="https://ttf-widget.pages.dev/assets/integrations/custom.js"]')
      
      if (script) {
        // If script exists, remove it first
        script.remove()
      }

      // Create new script
      const newScript = document.createElement('script')
      newScript.type = 'module'
      newScript.src = 'https://ttf-widget.pages.dev/assets/integrations/custom.js'

      newScript.onload = () => {
        this.InitializeWidget()
      }

      document.body.appendChild(newScript)
    },
    InitializeWidget () {
      const container = document.querySelector('#TTF_WIDGET_CONTAINER')

      if (!container) {
        console.error('TTF Widget container not found.')
        return
      }

      const apiKey = container.getAttribute('data-api-key')
      const productId = container.getAttribute('data-product-id')

      if (!apiKey || !productId) {
        console.error('Missing API key or Product ID.')
        // return
      }

      // Call the widget's initialization logic here if necessary
      // This depends on what the external JS provides
      // console.log("TTF Widget initialized with API Key:", apiKey, "and Product ID:", productId)
    }
  }
}
</script> 