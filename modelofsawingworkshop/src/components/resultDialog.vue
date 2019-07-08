<template>
  <v-dialog v-model="dialog" max-width="400">
    <v-card>
      <v-card-title
        class="headline"
      >Среднии потери за неделю = {{ parseFloat(loseMoneyNotWorkingSum1).toFixed(2) }}$</v-card-title>
      <v-card-text>Затраты на зарплату рабочим = {{ salaryWorker }}$</v-card-text>
      <v-card-text>Затраты на аренду машин = {{ priceOfArend }}$</v-card-text>
      <v-card-text>Среднии потери из-за простоя машин в неделю = {{ parseFloat(loseMoneyNotWorkingSum).toFixed(2) }}$</v-card-text>
      <v-card-text>Общие потери из-за простоя машин в целом = {{ parseFloat(loseMoneyNotWorkingSum2).toFixed(2) }}$</v-card-text>
      <v-card-text>Общие потери = {{ parseFloat(loseMoney).toFixed(2) }}$</v-card-text>
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
        loseMoneyNotWorkingSum: '',
        loseMoneyNotWorkingSum1: '',
        loseMoneyNotWorkingSum2: '',
        salaryWorker: '',
        priceOfArend: '',
        loseMoney: ''
    })
}
</script>