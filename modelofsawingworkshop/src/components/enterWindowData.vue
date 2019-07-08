<template>
  <v-dialog v-model="dialog" persistent max-width="600px">
    <template v-slot:activator="{ on }">
      <v-btn color="primary" dark v-on="on">ВВЕСТИ ДАННЫЕ</v-btn>
    </template>
    <v-card>
      <v-card-text>
        <v-container grid-list-md>
          <v-form ref="form">
            <v-text-field
              v-model="nowon"
              :counter="2"
              label="Количество рабочих мест и собственных машин"
              required
            ></v-text-field>
            <v-text-field
              v-model="spareMachine"
              :counter="2"
              label="Количество резервных швейных машин"
              required
            ></v-text-field>
            <v-text-field
              v-model="worker"
              :counter="2"
              label="Количество рабочих, которых стоит нанять"
              required
            ></v-text-field>
            <v-text-field
              v-model="weeks"
              :counter="3"
              label="Количество недель моделирования"
              required
            ></v-text-field>
            <v-text-field
              v-model="avgTimeWork"
              :counter="3"
              label="Среднее время работы машины(в часах)"
              required
            ></v-text-field>
            <v-text-field
              v-model="avgTimeFix"
              :counter="3"
              label="Среднее время ремонта машины(в часах)"
              required
            ></v-text-field>
            <v-text-field
              v-model="costArendPerDay"
              :counter="3"
              label="Стоимость аренды машины в день"
              required
            ></v-text-field>
            <v-text-field
              v-model="costSalaryPerDay"
              :counter="3"
              label="Зарплата рабочего в день"
              required
            ></v-text-field>
            <v-text-field
              v-model="costStopPerDay"
              :counter="3"
              label="Убыток из-за простоя одного рабочего места"
              required
            ></v-text-field>
          </v-form>
        </v-container>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" flat @click="dialog = false">Close</v-btn>
        <v-btn color="warning" @click="reset">Очистить форму</v-btn>
        <v-btn color="success" @click="run">Моделировать</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  data: () => ({
    dialog: false,
    nowon: "15", //Количество рабочих мест
    worker: "1", //Количество наёмных наладчиков
    spareMachine: "1", //Количество резервных машин
    weeks: "52", //Количество недель моделирования
    avgTimeWork: "157", //Среднее время работы одной швейной машины
    avgTimeFix: "7", //Среднее время ремонта одной машины
    costArendPerDay: "30", //Стоимость аренды одной машины в день
    costSalaryPerDay: "30", //Зарплата  в день
    costStopPerDay: "160" //Стоимость простоя машины за день
  }),
  methods: {
    run() {
      this.dialog = false;
      var newObj = {
        nowon: this.nowon,
        worker: this.worker,
        spareMachine: this.spareMachine,
        weeks: this.weeks,
        avgTimeWork: this.avgTimeWork,
        avgTimeFix: this.avgTimeFix,
        costArendPerDay: this.costArendPerDay,
        costSalaryPerDay: this.costSalaryPerDay,
        costStopPerDay: this.costStopPerDay
      }
      this.$emit('dataReadyFromChild', newObj)
    },
    reset() {
      this.$refs.form.reset();
    }
  }
};
</script>
