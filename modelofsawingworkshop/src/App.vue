<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span class="font-weight-light">МОДЕЛИРОВАНИЕ РАБОТЫ ШВЕЙНОГО ЦЕХА</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <enterWondowData @dataReadyFromChild="dataReadyGo"/>
      <v-btn color="success" @click="crear">Очистить массивы</v-btn>
      <v-btn color="success" @click="openResults">Открыть окно результатов</v-btn>
    </v-toolbar>
    <v-content>
      <HelloWorld
        :nowon="dataReady.nowon"
        :spareMachine="dataReady.spareMachine"
        :worker="dataReady.worker"
        :avgTimeWork="dataReady.avgTimeWork"
        :avgTimeFix="dataReady.avgTimeFix"
        :costArendPerDay="dataReady.costArendPerDay"
        :costSalaryPerDay="dataReady.costSalaryPerDay"
        :costStopPerDay="dataReady.costStopPerDay"
        ref="foo"
      />
    </v-content>
  </v-app>
</template>

<script>
import HelloWorld from "./components/HelloWorld";
import enterWondowData from "./components/enterWindowData"

export default {
  name: "App",
  components: {
    HelloWorld,
    enterWondowData
  },
  data: () => ({
    dataReady: {}
  }),
  methods: {
    async dataReadyGo(value) {
      this.dataReady = value
      await this.$refs.foo.run()
      this.$refs.foo.work(this.dataReady.weeks)
    },
    crear() {},
    openResults() {},
  }
};
</script>
