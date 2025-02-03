
    
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
        <q-btn flat label="Unload Script" @click="UnloadWidgetScript()" />
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
  watch: {
    modelValue(newVal) {
      if (newVal) {
        // this.LoadWidgetScript();
      } else {
        this.UnloadWidgetScript();
      }
    }
  },
  emits: ["update:modelValue"],
  mounted() {
    console.log("mounted");
    this.LoadWidgetScript();
  },
  created() {
    console.log("created");
  },
  unmounted() {
    // this.UnloadWidgetScript();
  },
  methods: {
    UnloadWidgetScript() {
      console.log("unloading script");
      const script = document.querySelector('script[data-script-source="ttf-widget"]')
      if (script) {
        console.log("script found");
        document.body.removeChild(script);
      }
    },
    LoadWidgetScript() {
      if (window.TTF_WIDGET) {
        // If the widget global object exists, just reinitialize it
        this.InitializeWidget();
        return;
      }

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