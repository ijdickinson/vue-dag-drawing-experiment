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
        <graph-edge
          v-for='edge in edges'
          :fromNode='edge.fromNode'
          :toNode='edge.toNode'
          :key='`edge-${edge.fromNode.id}-${edge.toNode.id}`'
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
import GraphEdge from './GraphEdge'

export default {
  name: 'graph-container',
  components: {
    GraphNode,
    GraphEdge
  },

  data: () => ({
    nodes: {
      1: { id: 1, x: 200, y: 100, w: 100, h: 50, label: 'frodo' },
      2: { id: 2, x: 450, y: 400, w: 100, h: 50, label: 'sam' },
      3: { id: 3, x: 600, y: 600, w: 100, h: 50, label: 'strider' }
    },
    edges: []
  }),

  mounted () {
    // create some test edges
    this.edges.push(
      { fromNode: this.nodes[1], toNode: this.nodes[2] }
    )
  },

  methods: {
    onMove ({ x, y, id }) {
      const node = this.nodes[id]
      const updatedNode = Object.assign({}, node, { x, y })

      if (node) {
        this.$set(this.nodes, node.id, updatedNode)
        this.updateAffectedEdges(updatedNode)
      }
    },

    updateAffectedEdges (node) {
      for (const edge of this.edges) {
        if (edge.fromNode.id === node.id) {
          this.$set(edge, 'fromNode', node)
        } else if (edge.toNode.id === node.id) {
          this.$set(edge, 'toNode', node)
        }
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
