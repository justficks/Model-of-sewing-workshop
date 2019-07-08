<template>
  <v-container grid-list-md text-xs-center>
    <firstDT/>
  </v-container>
</template>

<script>
import firstDT from "../components/firtsDT";

export default {
  components: {
    firstDT
  },
  props: [
    "nowon",
    "spareMachine",
    "worker",
    "weeks",
    "avgTimeWork",
    "avgTimeFix",
    "costArendPerDay",
    "costSalaryPerDay",
    "costStopPerDay"
  ],
  data: () => ({
  })
};
</script>

<style>
</style>
