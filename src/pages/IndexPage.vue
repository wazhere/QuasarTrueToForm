<template>
  <q-page >
    
<div class="flex flex-center" style="margin-top: 100px;">
    <q-btn icon="check_circle" color="primary" label="Open Modal No Layout" @click="showDialogNoLayout = true" />&nbsp;
    <q-btn icon="error" color="primary" label="Open Modal Layout Container" @click="showDialogLayoutContainer = true" />&nbsp;
    <q-btn icon="check_circle" color="primary" label="Open Modal Layout No Container" @click="showDialogLayoutNoContainer = true" />&nbsp;
    <lazy-true-to-form-dialog-no-layout v-model="showDialogNoLayout" v-if="showDialogNoLayout" />
    <lazy-true-to-form-dialog-layout-container v-model="showDialogLayoutContainer" v-if="showDialogLayoutContainer" />
    <lazy-true-to-form-dialog-layout-no-container v-model="showDialogLayoutNoContainer" v-if="showDialogLayoutNoContainer" />
  </div>
</q-page>
</template>

<script>
import { defineAsyncComponent } from 'vue'

export default {
  name: 'IndexPage',
  components: {
    LazyTrueToFormDialogNoLayout: defineAsyncComponent(() => 
      import('src/components/TrueToFormDialogNoLayout.vue')
    ),
    LazyTrueToFormDialogLayoutContainer: defineAsyncComponent(() => 
      import('src/components/TrueToFormDialogLayoutContainer.vue')
    ),
    LazyTrueToFormDialogLayoutNoContainer: defineAsyncComponent(() => 
      import('src/components/TrueToFormDialogLayoutNoContainer.vue')
    )
  },
  data () {
    return {
      showDialogLayoutContainer: false,
      showDialogLayoutNoContainer: false,
      showDialogNoLayout: false
    }
  }
}
</script>
