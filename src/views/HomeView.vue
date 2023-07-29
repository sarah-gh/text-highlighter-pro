<template>
  <div class="home">
    <ColorfulTextEditor></ColorfulTextEditor>
  </div>
</template>

<script>
import ColorfulTextEditor from '@/components/ColorfulTextEditor.vue'

export default {
  name: 'HomeView',
  components: {
    ColorfulTextEditor,
  }
}
</script>
