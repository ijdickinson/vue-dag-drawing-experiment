<template>
  <g>
    <marker
      id='arrow'
      viewBox='0 0 10 10'
      refX='5' refY='5'
      markerWidth='6'
      markerHeight='6'
      orient='auto-start-reverse'
    >
      <path d="M 0 0 L 10 5 L 0 10 z" />
    </marker>
    <line
      :x1='startX'
      :y1='startY'
      :x2='endX'
      :y2='endY'
      class='c-graph-edge'
      marker-end='url("#arrow")'
    />

  </g>
</template>

<script>
export default {
  name: 'graph-edge',

  props: {
    fromNode: {
      type: Object,
      required: true
    },
    toNode: {
      type: Object,
      required: true
    }
  },

  data: () => ({
    fromNodePoints: null,
    toNodePoints: null
  }),

  computed: {
    startX () {
      return this.closestHandlePair.fromHandle.x
    },

    startY () {
      return this.closestHandlePair.fromHandle.y
    },

    endX () {
      return this.closestHandlePair.toHandle.x
    },

    endY () {
      return this.closestHandlePair.toHandle.y
    },

    /* Cartesian product of the from handles and the to handles */
    handlePairs () {
      const fromHandles = this.fromNodePoints.handles
      const toHandles = this.toNodePoints.handles

      const pairs = []
      for (const fromHandle of fromHandles) {
        for (const toHandle of toHandles) {
          pairs.push({ fromHandle, toHandle })
        }
      }

      return pairs
    },

    closestHandlePair () {
      let closestDist = null
      let closestPair = null

      for (const pair of this.handlePairs) {
        const d = this.distance(pair)

        if (!closestPair || d < closestDist) {
          closestDist = d
          closestPair = pair
        }
      }

      return closestPair
    }
  },

  watch: {
    fromNode: {
      handler: function (node) {
        this.fromNodePoints = this.nodePoints(node)
      },
      immediate: true
    },
    toNode: {
      handler: function (node) {
        this.toNodePoints = this.nodePoints(node)
      },
      immediate: true
    }
  },

  methods: {
    nodePoints (node) {
      if (!node) {
        return {}
      }

      const x = node.x + node.displacement.x
      const y = node.y + node.displacement.y

      return {
        handles: [
          { x: x + node.w, y: y }, // north
          { x: x + node.w * 2, y: y + node.h }, // east
          { x: x + node.w, y: y + node.h * 2 }, // south
          { x: x, y: y + node.h } // west
        ]
      }
    },

    /* Pythagorean distance between two points */
    distance ({ fromHandle, toHandle }) {
      const dx = fromHandle.x - toHandle.x
      const dy = fromHandle.y - toHandle.y

      return Math.sqrt(dx * dx + dy * dy)
    }
  }
}
</script>

<style>
.c-graph-edge {
  stroke: black;
}
</style>
