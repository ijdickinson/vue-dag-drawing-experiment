<template>
  <div>
    <graph-container></graph-container>
  </div>
</template>

<script>
import GraphContainer from '@/components/GraphContainer.vue'

export default {
  name: 'dag-container',
  components: {
    // Womble
    GraphContainer
  }
}
</script>
