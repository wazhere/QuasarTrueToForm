
    
<template>
  <q-dialog
    :model-value="modelValue"
    @update:model-value="$emit('update:modelValue', $event)"
  >
    <q-card>
      <q-card-section>
        <div class="text-h6">TTF Example TH134 Modal</div>
      </q-card-section>

      <q-card-section>
        <div style="background-color: red; color: white">
          TTF_WIDGET_CONTAINER
        </div>
        <div style="border: solid 1px red; height: 80px;" class="text-center"> 
          (Green is the container)
          <div class="row justify-center" style="padding-top: 10px;">
          <div
            ref="ttfWidgetContainer"
            style="border: solid 2px green; height: 40px; min-width: 500px;"
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
      <q-card-section>
        <div style="min-width: 500px; text-align: center; padding: 30px 0; background-color: #eee;">
          <h2 style="font-size: 20px; line-height: 20px; margin: 0 0 10px 0; padding:0">"Console" Logs</h2>
          {{ Logs }}
        </div>
      </q-card-section>

      <q-card-actions align="right">
        <q-btn flat label="Unload TTF Script" @click="UnloadWidgetScript()" />
        <q-btn flat label="Close" color="primary" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script>
export default {
    name: 'TrueToForm',
    mounted () {
        this.LoadWidgetScript()
    },
    data () {
      return {
        Logs: []
      }
    },
    methods: {
      ConsoleLog (message) {
        this.Logs.push(message)
      },
      UnloadWidgetScript () {
        document.querySelector('script[data-script-source="ttf-widget"]').remove()
      },
        LoadWidgetScript () {
                this.ConsoleLog("LoadWidgetScript function called")
                // Check if the script is already loaded
                if (document.querySelector('script[src="https://ttf-widget.pages.dev/assets/integrations/widget.js"]')) {
                    this.ConsoleLog("Widget script already loaded")
                    this.InitializeWidget()
                    return
                }

                this.ConsoleLog("Loading widget script")
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
              const _this = this
              _this.ConsoleLog("Initializing widget")
// Wait until all scripts are loaded and the DOM is fully parsed
  if (window.mountTTFWidget) {
    // the container should be any valid HTML element
    //const container = document.getElementById("TTF_WIDGET_CONTAINER"); // feel free to query the container in any other ways
    const container = this.$refs.ttfWidgetContainer
    this.ConsoleLog("Existing widget found")
    this.ConsoleLog(JSON.stringify(container))
    window.mountTTFWidget(container, {
      // Required
      apiKey: "i8ZWxd3vHEgf8vczXE5N",
      productId: "TH134",

      // Optional customizations
      widgetEvents: {
        // Callback when user opens/closes the modal
        onOpenChange(isOpen) {
          _this.ConsoleLog(`widget is open: ${isOpen}`);
        },
        
        // Callback when user selects a size within the TrueToForm fit prediction widget
        onSizeSelection: ({ size, isRecommendedSize }) => {
          _this.ConsoleLog("Selected size:", size, "Is recommended:", isRecommendedSize);
        },

        // Callback when a size is recommended
        onSizeRecommendation: (size) => {
          _this.ConsoleLog("Recommended size:", size);
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
