<template>
  <v-card width="800" height="160">
      <v-card-text>
            <section class="title">
              <span class="green--text">{{correctAnswers}}</span> / <span class="red--text">{{incorrectAnswers}}</span>
            </section>
            <section ref="op" v-if="op" class="text-center display-4">{{opLabel}}</section>
      </v-card-text>
  </v-card>
</template>

<script>
export default {
    props: ['op', 'correctAnswers', 'incorrectAnswers'],
    computed: {
        opLabel () {
            return `${this.op.digit1} ${this.op.opChar} ${this.op.digit2}`;
        }
    }
}
</script>

<style></style>
