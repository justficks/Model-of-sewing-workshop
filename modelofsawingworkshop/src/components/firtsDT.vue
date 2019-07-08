<template>
  <v-container>
    <v-data-table :headers="headers" :items="desserts" :loading="loading" class="elevation-1">
      <template v-slot:items="props">
        <td class="text-xs-right">{{ props.item.countWeeks }}</td>
        <td class="text-xs-right">{{ props.item.avgStoping }}</td>
        <td class="text-xs-right">{{ props.item.avgReserve }}</td>
        <td class="text-xs-right">{{ props.item.avgWorking }}</td>
        <td class="text-xs-right">{{ props.item.avgFixing }}</td>
        <td class="text-xs-right">{{ props.item.avgLoseMoneyNotWorking }}</td>
      </template>
    </v-data-table>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    loading: false,
    headers: [
      { text: "Недель прошло", value: "countWeeks", align: "center" },
      { text: "Avg простаивающих", value: "avgStoping", align: "center" },
      { text: "Avg в резерве", value: "avgReserve", align: "center" },
      { text: "Avg в работе", value: "avgWorking", align: "center" },
      { text: "Avg в ремонте", value: "avgFixing", align: "center" },
      {
        text: "Потери из-за простоя в неделю",
        value: "avgLoseMoneyNotWorking",
        align: "center"
      }
    ],
    desserts: [
      {
        countWeeks: "",
        avgStoping: "",
        avgReserve: "",
        avgWorking: "",
        avgFixing: "",
        avgLoseMoneyNotWorking: ""
      }
    ]
  })
};
</script>
