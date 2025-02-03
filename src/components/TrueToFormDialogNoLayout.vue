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
    mounted () {
      this.LoadWidgetScript()
    },
    methods: {
      LoadWidgetScript () {
        // Check if the script is already loaded
        if (document.querySelector('script[src="https://ttf-widget.pages.dev/assets/integrations/custom.js"]')) {
          this.InitializeWidget()
          return
        }
  
        // Dynamically create a script tag and load the external JS file
        const script = document.createElement('script')
        script.type = 'module'
        script.src = 'https://ttf-widget.pages.dev/assets/integrations/custom.js'
  
        // Once the script is loaded, initialize the widget
        script.onload = () => {
          this.InitializeWidget()
        }
  
        // Append the script to the document body
        document.body.appendChild(script)
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