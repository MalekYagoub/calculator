<template>
    <v-btn width="40" class="title" @click="numberClick" :disabled="incorrectUserResults.includes(number)">
      {{number}}
    </v-btn>
</template>

<script>
export default {
    props: ['number', 'incorrectUserResults', 'numberChoicesEnabled'],
    methods: {
        numberClick () {
            this.$emit('emit-number-click', this.number);
        }
    }
}
</script>

<style>
    .incorrect-number {
        transition: 0.1s;
        background-color: #FF5252 !important;
    }

    .correct-number {
        transition: 0.1s;
        background-color: #69F0AE !important;
    }
</style>
