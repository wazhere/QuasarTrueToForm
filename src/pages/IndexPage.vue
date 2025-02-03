<template>
  <q-page class="flex flex-center">
    <q-btn color="primary" label="Open Modal" @click="showDialog = true" />
    <lazy-true-to-form-dialog v-model="showDialog" v-if="showDialog" />
  </q-page>
</template>

<script>
import { defineAsyncComponent } from 'vue'

export default {
  name: 'IndexPage',
  components: {
    LazyTrueToFormDialog: defineAsyncComponent(() => 
      import('src/components/TrueToFormDialog.vue')
    )
  },
  data () {
    return {
      showDialog: false
    }
  }
}
</script>
