<template>
  <q-dialog
    :model-value="modelValue"
    @update:model-value="$emit('update:modelValue', $event)"
    @before-show="InitializeWidget"
    @before-hide="UnloadWidgetScript"
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
  name: "TrueToFormDialog",
  props: {
    modelValue: {
      type: Boolean,
      required: true
    }
  },
  emits: ["update:modelValue"],
  mounted() {
    this.Log("Vue: mounted", "orange");
    this.LoadWidgetScript();
  },
  activated() {
    this.Log("Vue: activated from keep-alive", "blue");
    // Don't reload the script if it already exists
    if (!document.querySelector('script[data-script-source="ttf-widget"]')) {
      this.LoadWidgetScript();
    } else {
      this.InitializeWidget();
    }
  },
  deactivated() {
    this.Log("Vue: deactivated from keep-alive", "blue");
    // Don't unload the script when component is cached
  },
  unmounted() {
    this.Log("Vue: unmounted", "orange");
    // Only unload if we're really destroying the component
    this.UnloadWidgetScript();
  },
  methods: {
    Log(message, color = "green") {
      this.$q.notify({
        message: message,
        color: color,
        group: false,
        position: 'bottom-right'
      });
    },
    UnloadWidgetScript() {
      this.Log("UnloadWidgetScript called", "orange");
      const script = document.querySelector('script[data-script-source="ttf-widget"]')
      if (script) {
        this.Log("UnloadWidgetScript: script found, removing", "green");
        document.body.removeChild(script);
      } else {
        this.Log("UnloadWidgetScript: script not found", "red");
      }
    },
    LoadWidgetScript() {
      this.Log("LoadWidgetScript called", "orange");
      /*
      if (window.TTF_WIDGET) {
        // If the widget global object exists, just reinitialize it
        this.InitializeWidget();
        return;
      }
      */

      const script = document.createElement("script");
      script.type = "module";
      script.src = "https://ttf-widget.pages.dev/assets/integrations/custom.js";
      script.setAttribute("data-script-source", "ttf-widget");

      script.onload = () => {
        this.InitializeWidget();
      };

      document.body.appendChild(script);
    },
    InitializeWidget() {
      const container = document.querySelector("#TTF_WIDGET_CONTAINER");

      if (!container) {
        console.error("TTF Widget container not found.");
        return;
      }

      if (window.TTF_WIDGET) {
        // If the widget global already exists, reinitialize it
        try {
          window.TTF_WIDGET.init({
            container: container,
            apiKey: container.getAttribute("data-api-key"),
            productId: container.getAttribute("data-product-id")
          });
        } catch (err) {
          console.error("Error reinitializing TTF Widget:", err);
        }
        return;
      }

      // If the script was loaded but the widget isn't ready yet, wait for it
      setTimeout(() => {
        if (window.TTF_WIDGET) {
          this.InitializeWidget();
        }
      }, 500);
    }
  }
};
</script>      