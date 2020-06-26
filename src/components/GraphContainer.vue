<template>
  <div class='c-graph-container'>
    <header>
      <nav>
        the header
      </nav>
    </header>
    <main>
      <svg
        xmlns='http://www.w3.org/2000/svg'
        xmlns:xlink='http://www.w3.org/1999/xlink'
      >
        <graph-node
          v-for='node in nodes'
          :nodeData='node'
          :key='node.id'
          @move='onMove'
        />
      </svg>
    </main>
    <footer>
      the footer
    </footer>
  </div>
</template>

<script>
import GraphNode from './GraphNode'

export default {
  name: 'graph-container',
  components: {
    GraphNode
  },

  data: () => ({
    nodes: [
      { id: 1, x: 200, y: 100, w: 100, h: 50, label: 'frodo' },
      { id: 2, x: 450, y: 400, w: 100, h: 50, label: 'sam' },
      { id: 3, x: 600, y: 600, w: 100, h: 50, label: 'strider' }
    ],
    edges: [

    ]
  }),

  methods: {
    onMove ({ x, y, id }) {
      const node = this.nodes.find(node => node.id === id)
      if (node) {
        node.x = x
        node.y = y
      }
    }
  }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
.c-graph-container {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.c-graph-container header, .c-graph-container footer {
  height: 3em;
  background-color: #999;
  flex-grow: 0;
}

.c-graph-container main {
  flex-grow: 1;
}

.c-graph-container main svg {
  height: 100%;
  width: 100%;
}

</style>
