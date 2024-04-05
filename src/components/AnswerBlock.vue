<template>
  <div v-if="text">
    <h3>{{ title }}</h3>
    <div class="card">
      <pre>{{ text }}</pre>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    text: {
      type: String,
      default: "",
    },
    title: {
      type: String,
      default: "",
    },
  },

  methods: {},
};
</script>
