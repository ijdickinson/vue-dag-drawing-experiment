<template>
  <line
    :x1='startX'
    :y1='startY'
    :x2='endX'
    :y2='endY'
    class='c-graph-edge'
  />
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
      return this.closestHandlePair.closestPair.fromHandle.x
    },

    startY () {
      return this.closestHandlePair.closestPair.fromHandle.y
    },

    endX () {
      return this.closestHandlePair.closestPair.toHandle.x
    },

    endY () {
      return this.closestHandlePair.closestPair.toHandle.y
    },

    /* Cartesian product of the from handles and the to handles */
    handlePairs () {
      const fromHandles = this.fromNodePoints.handles
      const toHandles = this.toNodePoints.handles

      return fromHandles.map(fromHandle => {
        return toHandles.map(toHandle => ({ fromHandle, toHandle }))
      }).flat()
    },

    closestHandlePair () {
      const vm = this

      return this.handlePairs.reduce(
        function ({ distance, closestPair }, pair) {
          if (!closestPair) {
            return { distance: vm.distance(pair), closestPair: pair }
          } else {
            const d = vm.distance(pair)
            if (d < distance) {
              return { distance: d, closestPair: pair }
            }

            return { distance, closestPair }
          }
        },
        {}
      )
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

      return {
        centre: {
          x: node.x + node.w,
          y: node.y + node.h
        },
        handles: [
          { x: node.x + node.w, y: node.y }, // north
          { x: node.x + node.w * 2, y: node.y + node.h }, // east
          { x: node.x + node.w, y: node.y + node.h * 2 }, // south
          { x: node.x, y: node.y + node.h } // west
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
