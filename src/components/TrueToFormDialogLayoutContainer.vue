
    
<template>
  <q-dialog
    :model-value="modelValue"
    @update:model-value="$emit('update:modelValue', $event)"
    @before-show="InitializeWidget"
  >
  <q-layout ref="TabRef" container class="bg-lm-lightest">
    <q-card>
      <q-card-section>
        <div class="text-h6">TTF Example TH134 Modal</div>
      </q-card-section>

      <q-card-section>
        <div style="background-color: red; color: white">
          TTF_WIDGET_CONTAINER
        </div>
        <div style="border: solid 1px red; height: 40px;"> 
          <div class="row justify-center" style="padding-top: 10px;">
          <div
              id="TTF_WIDGET_CONTAINER"
              data-product-id="TH134"
              data-api-key="i8ZWxd3vHEgf8vczXE5N"
          ></div>
        </div>
        </div>
      </q-card-section>

      <q-card-section>
        <div style="min-width: 500px; text-align: center; padding: 30px 0; background-color: #eee;">EXISTING TH134 CHOOSE SIZE STUFF</div>
      </q-card-section>

      <q-card-actions align="right">
        <q-btn flat label="Unload TTF Script" @click="UnloadWidgetScript()" />
        <q-btn flat label="Close" color="primary" v-close-popup />
      </q-card-actions>
    </q-card>
    </q-layout>
  </q-dialog>
</template>

<script>
export default {
    name: 'TrueToForm',
    mounted () {
        this.LoadWidgetScript()
    },
    methods: {
      UnloadWidgetScript () {
        document.querySelector('script[data-script-source="ttf-widget"]').remove()
      },
        LoadWidgetScript () {
                // Check if the script is already loaded
                if (document.querySelector('script[src="https://ttf-widget.pages.dev/assets/integrations/widget.js"]')) {
                    this.InitializeWidget()
                    return
                }

                // Dynamically create a script tag and load the external JS file
                const script = document.createElement('script')
                script.type = 'module'
                script.src = 'https://ttf-widget.pages.dev/assets/integrations/widget.js'
                script.setAttribute('data-script-source', 'ttf-widget')

                // Once the script is loaded, initialize the widget
                script.onload = () => {
                    this.InitializeWidget()
                }

                // Append the script to the document body
                document.body.appendChild(script)
            },
            InitializeWidget () {
// Wait until all scripts are loaded and the DOM is fully parsed
  if (window.mountTTFWidget) {
    // the container should be any valid HTML element
    const container = document.getElementById("TTF_WIDGET_CONTAINER"); // feel free to query the container in any other ways

    window.mountTTFWidget(container, {
      // Required
      apiKey: "i8ZWxd3vHEgf8vczXE5N",
      productId: "TH134",

      // Optional customizations
      widgetEvents: {
        // Callback when user opens/closes the modal
        onOpenChange(isOpen) {
          console.log(`widget is open: ${isOpen}`);
        },
        
        // Callback when user selects a size within the TrueToForm fit prediction widget
        onSizeSelection: ({ size, isRecommendedSize }) => {
          console.log("Selected size:", size, "Is recommended:", isRecommendedSize);
        },

        // Callback when a size is recommended
        onSizeRecommendation: (size) => {
          console.log("Recommended size:", size);
        },
      },
      widgetStyles: {
        // Customize button appearance
        button: {
          fontSize: "14px",
          color: "#7e7e7e",
          fontWeight: 400,
          lineHeight: "20px",
          // Customize logo color
          logoColor: "#000",
        },
      },
    });
  }
                
              
            }
    }
}
</script>
