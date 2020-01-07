<template>
    <v-btn
        width="40"
        class="headline"
        @click="emitChangeSelectedType"
        :disabled="selectedType === calculationType"
    >
        {{ calculationType }}
    </v-btn>
</template>

<script>
export default {
  props: ["calculationType", "selectedType"],
  methods: {
      emitChangeSelectedType () {
          this.$emit('emit-change-selected-type', this.calculationType);
      }
  }
};
</script>

<style></style>
