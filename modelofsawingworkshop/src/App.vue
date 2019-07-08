<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span class="font-weight-light">МОДЕЛИРОВАНИЕ РАБОТЫ ШВЕЙНОГО ЦЕХА</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <enterWondowData @dataReadyFromChild="dataReadyGo"/>
      <v-btn color="warning" @click="cleare">Очистить массивы</v-btn>
      <v-btn color="success" @click="openResults">Открыть окно результатов</v-btn>
    </v-toolbar>
    <v-content>
      <HelloWorld ref="foo"/>
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
  methods: {
    async dataReadyGo(value) {
      await this.$refs.foo.run(value)
      this.$refs.foo.work(value.weeks)
    },
    cleare() {
      this.$refs.foo.clearall()
    },
    openResults() {
      this.$refs.foo.openResults()
    },
  }
};
</script>
