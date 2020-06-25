<template>
  <g
    transform='translate(0,0)'
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
export default {
  name: 'graph-node',
  props: {
    nodeData: {
      type: Object,
      required: true
    }
  },
  computed: {
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
  }
}
</script>

<style>
.c-graph-node {
  fill: #99c;
  stroke: #336;
}

.c-graph-node__label {
  fill: black;
  stroke: black;
}
</style>
