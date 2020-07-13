<template>
  <g
    :transform='svgLocation'
    ref='svgG'
    class='c-svg-g'
  >
    <ellipse
      :cx='centreX'
      :cy='centreY'
      :rx='radiusX'
      :ry='radiusY'
      class='c-graph-node'
    />
    <text
      class='c-graph-node__label'
      :x='textX'
      :y='textY'
      dominant-baseline='middle'
      text-anchor='middle'
    >
      <tspan>{{ label }}</tspan>
    </text>
  </g>
</template>

<script>
import interact from 'interactjs'

export default {
  name: 'graph-node',

  props: {
    nodeData: {
      type: Object,
      required: true
    }
  },

  data: () => ({
    displacement: {
      x: 0,
      y: 0
    }
  }),

  computed: {
    svgLocation () {
      const x = this.nodeData.x + this.nodeData.displacement.x
      const y = this.nodeData.y + this.nodeData.displacement.y
      return `translate(${x},${y})`
    },

    centreX () {
      return this.nodeData.w
    },

    centreY () {
      return this.nodeData.h
    },

    radiusX () {
      return this.nodeData.w
    },

    radiusY () {
      return this.nodeData.h
    },

    label () {
      return this.nodeData.label || 'A node with no name'
    },

    textX () {
      return this.nodeData.w
    },

    textY () {
      return this.nodeData.h
    }
  },

  mounted () {
    this.initInteractJs()
  },

  methods: {
    initInteractJs () {
      const interactive = interact(this.$refs.svgG)
      interactive.draggable({
        origin: 'self',
        inertia: false,
        modifiers: [
          interact.modifiers.restrict({
            restriction: 'parent'
          })
        ],
        listeners: {
          move: this.onElementMove,
          end: this.onElementMoveEnd
        }
      })
    },

    resetDisplacement () {
      this.displacement = {
        x: 0,
        y: 0
      }
    },

    onElementMove (event) {
      const { x, y } = event.page
      const { x0, y0 } = event

      this.$emit('move',
        {
          x: x - x0,
          y: y - y0,
          id: this.nodeData.id
        })
    },

    onElementMoveEnd (event) {
      this.$emit('move-end', { id: this.nodeData.id })
    }
  }
}
</script>

<style>
.c-svg-g {
  touch-action: none;
}

.c-graph-node {
  fill: #99c;
  stroke: #336;
}

.c-graph-node__label {
  fill: black;
  stroke: black;
}
</style>
