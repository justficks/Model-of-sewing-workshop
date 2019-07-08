<template>
  <v-dialog v-model="dialog" max-width="430">
    <v-card >
      <v-card-title><h2>Результаты</h2></v-card-title>
      <table>
        <!--ряд с ячейками заголовков-->
        <tr>
          <td class="rb">Среднии потери за неделю</td>
          <td>{{ parseFloat(loseMoneyNotWorkingSum1).toFixed(2) }}$</td>
        </tr>
        <tr>
            <td class="rb">Затраты на зарплату рабочим</td>
            <td>{{ salaryWorker }}$</td>
        </tr>
        <tr>
            <td class="rb">Затраты на аренду машин</td>
            <td>{{ priceOfArend }}$</td>
        </tr>
        <tr>
            <td class="rb">Среднии потери из-за простоя машин в неделю</td>
            <td>{{ parseFloat(loseMoneyNotWorkingSum).toFixed(2) }}$</td>
        </tr>
        <tr>
            <td class="rb">Общие потери из-за простоя машин в целом</td>
            <td>{{ parseFloat(loseMoneyNotWorkingSum2).toFixed(2) }}$</td>
        </tr>
        <tr>
            <td class="rb"><b>Общие потери</b></td>
            <td><b>{{ parseFloat(loseMoney).toFixed(2) }}$</b></td>
        </tr>
        <!--ряд с ячейками тела таблицы-->
      </table>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="green darken-1" flat="flat" @click="dialog = false">Закрыть</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  data: () => ({
    dialog: false,
    loseMoneyNotWorkingSum: "",
    loseMoneyNotWorkingSum1: "",
    loseMoneyNotWorkingSum2: "",
    salaryWorker: "",
    priceOfArend: "",
    loseMoney: ""
  })
};
</script>

<style>
.rb {
    padding: 10px;
    border-right: 2px solid lightgrey
}
tr:first-child {
    border-right: 2px solid black
}
</style>
