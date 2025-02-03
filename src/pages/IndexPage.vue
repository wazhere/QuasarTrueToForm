<template>
  <q-page class="flex flex-center">
    <q-btn color="primary" label="Open Modal Layout" @click="showDialogLayout = true" />&nbsp;
    <q-btn color="primary" label="Open Modal No Layout" @click="showDialogNoLayout = true" />
    <keep-alive>
      <lazy-true-to-form-dialog-layout v-model="showDialogLayout" v-if="showDialogLayout" />
      <lazy-true-to-form-dialog-no-layout v-model="showDialogNoLayout" v-if="showDialogNoLayout" />
    </keep-alive>
  </q-page>
</template>

<script>
import { defineAsyncComponent } from 'vue'

export default {
  name: 'IndexPage',
  components: {
    LazyTrueToFormDialogLayout: defineAsyncComponent(() => 
      import('src/components/TrueToFormDialogLayout.vue')
    ),
    LazyTrueToFormDialogNoLayout: defineAsyncComponent(() => 
      import('src/components/TrueToFormDialogNoLayout.vue')
    )
  },
  data () {
    return {
      showDialogLayout: false,
      showDialogNoLayout: false
    }
  }
}
</script>
