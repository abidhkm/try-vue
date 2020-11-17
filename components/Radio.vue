<template>
  <div>
    <v-card
      v-for="option in options"
      :key="option"
      class="radio"
      @click="$emit('answer-select', option)"
      v-bind:class="option === value ? 'selected' : ''"
    >
      <v-card-text>
        {{ option }}
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
export default {
  props: ['options', 'value'],
}
</script>

<style scoped>
.radio {
  margin: 16px 0;
}
.selected {
  border: 1px solid white;
}
</style>