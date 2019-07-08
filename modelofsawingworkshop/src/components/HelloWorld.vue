<template>
  <v-container grid-list-md text-xs-center>
    <firstDT ref="firstDT"/>
    <v-layout>
      <v-flex xs6></v-flex>
      <v-flex xs6></v-flex>
    </v-layout>
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
    workplace: [],
    spareplace: [],
    workers: [],
    countArendMachine: 0,
  }),
  methods: {
    run() {
      this.$refs.firstDT.loading = true;
      for (var i = 0; i < this.nowon; i++) {
        var newMachine = {
          id: i,
          toWork: "inWork",
          timeWork: this.exDistribution(this.avgTimeWork),
          timeFix: this.exDistribution(this.avgTimeFix),
          startFix: "",
          endFix: ""
        }; //157
        this.workplace.push(newMachine);
      }
      for (var j = i; j < parseInt(this.spareMachine) + i; j++) {
        var newSpareMachine = {
          id: j,
          toWork: "inReserve",
          timeWork: this.exDistribution(this.avgTimeWork),
          timeFix: this.exDistribution(this.avgTimeFix)
        };
        this.spareplace.push(newSpareMachine);
        this.countArendMachine++;
      }
      for (var x = 0; x < this.worker; x++) {
        var newWorker = { id: x, isFree: true, whichM: "" };
        this.workers.push(newWorker);
      }
    },

    exDistribution(L) {
      var rnd = this.getRandom(0, 1); // Случайное число в диапозоне от 0 до 1
      var result = Math.log(rnd) / -(1 / L);
      return Math.round(result); // Округление числа
    },
    getRandom(min, max) {
      return Math.random() * (max - min) + min;
    }
  }
};
</script>

