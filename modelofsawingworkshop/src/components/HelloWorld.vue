<template>
  <v-container grid-list-md text-xs-center>
    <firstDT ref="firstDT" />
    <v-layout>
      <v-flex xs6>
        <v-data-table
        :headers="headers2"
        :items="workplace"
        class="elevation-1"
        >
          <template v-slot:items="props">
            <td class="text-xs-right">{{ props.item.id }}</td>
            <td class="text-xs-right">{{ props.item.toWork }}</td>
            <td class="text-xs-right">{{ props.item.timeWork }}</td>
            <td class="text-xs-right">{{ props.item.timeFix }}</td>
            <td class="text-xs-right">{{ props.item.endFix }}</td>
          </template>
        </v-data-table>
      </v-flex>
      <v-flex xs6>
        <v-layout row wrap pa-4>
          <v-flex xs6 id="remontCeh" class="item elevation-5" px-4>
            <h2>Машины в резерве</h2>
            <v-layout row wrap>
              <div
                v-for="place in COMworkplaceReserve"
                :key="place.id"
                id="spareplace"
                class="item elevation-5"
              >{{ place.id }}</div>
            </v-layout>
          </v-flex>
          <v-flex xs6 id="remontCeh" class="item elevation-5">
            <h2>Работающие машины</h2>
            <v-layout row wrap>
              <div
                v-for="place in COMworkplaceWork"
                :key="place.id"
                id="workplace"
                class="item elevation-5"
              >{{ place.id }}</div>
            </v-layout>
          </v-flex>
          <v-flex xs6 id="remontCeh" class="item elevation-5">
            <h2>Машины в очереди на ремонт</h2>
            <v-layout row wrap>
              <div
                v-for="place in COMworkplaceBroke"
                :key="place.id"
                id="waitRemont"
                class="item elevation-5"
              >{{ place.id }}</div>
            </v-layout>
          </v-flex>
          <v-flex xs6 id="remontCeh" class="item elevation-5">
            <h2>Ремонтный цех</h2>
            <v-layout row wrap py-1>
              <v-flex xs6>
                <div
                  v-for="place in workers"
                  :key="place.id"
                  id="remont"
                  class="item elevation-5"
                >Рабочий №{{ place.id }}</div>
              </v-flex>
              <v-flex xs6>
                <h3>Машина на ремонте</h3>
                <v-layout column wrap>
                  <div
                    v-for="place in COMworkplaceFix"
                    :key="place.id"
                    id="waitRemont"
                    class="item elevation-5"
                  >{{ place.id }}</div>
                </v-layout>
              </v-flex>
            </v-layout>
          </v-flex>
        </v-layout>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import firstDT from "../components/firtsDT";

export default {
  components: {
    firstDT
  },
  data: () => ({
    interval: null,
    enteredData: {},

    workplace: [],
    spareplace: [],
    workers: [],
    stopingArr: [],
    reserveArr: [],
    workingArr: [],
    fixingArr: [],
    loseMoneyNotWorking: 0,
    avgLoseMoneyNotWorking: 0,
    avgLoseMoneyNotWorkingWeeks: [],

    countArendMachine: 0,
    countWeeks: 0,
    time: 1,
    salaryWorker: '', //Зарплата рабочих
    priceOfArend: '', //Стоимость аренды
    loseMoney: '', //Убыток из-за простоя одного рабочего места

    headers2: [
      { text: 'id', value: 'id'},
      { text: 'Состояние', value: 'toWork' },
      { text: 'Время работы', value: 'timeWork' },
      { text: 'Время ремонта', value: 'timeFix' },
      { text: 'Окончание ремонта', value: 'endFix' }
    ]
  }),
  methods: {
    work(countWeeks) {
      this.countWeeks = parseInt(countWeeks)
      this.interval = setInterval(this.start, 10);
    },
    start() {
      this.$refs.firstDT.loading = true;

      if (this.time <= this.countWeeks * 40) {
        //alert('Зашли в цикл. Время: ' + this.time)
        this.$refs.firstDT.desserts[0].countWeeks = Math.floor(this.time / 40);
        for (var i = 0; i < this.workplace.length; i++) {
          //Если рабочая машина сломалась, то переводим резервную машину в рабочий массив
          if (
            this.workplace[i].timeWork < this.time &&
            this.workplace[i].toWork != "inFix" &&
            this.workplace[i].toWork != "inReserve"
          ) {
            this.workplace[i].toWork = "broke";
            this.workplace[i].startFix = this.time;
            this.workplace[i].endFix = this.time + this.workplace[i].timeFix;
            var workingMachineLength = this.workplace.filter(
              x => x.toWork == "inWork"
            );
            if (
              this.spareplace.length != 0 &&
              workingMachineLength.length <= this.enteredData.nowon
            ) {
              // Вторая проверка, для того чтобы не было лишних работающих машин
              var reverse = {
                id: this.spareplace[0].id,
                toWork: "inWork",
                timeWork: this.spareplace[0].timeWork,
                timeFix: this.spareplace[0].timeFix
              };
              this.workplace.push(reverse);
              this.spareplace.splice(0, 1);
            }
          }
        }
        for (var x = 0; x < this.workplace.length; x++) {
          if (this.workplace[x].toWork == "broke") {
            var j;
            for (j = 0; j < this.workers.length; j++) {
              if (this.workers[j].isFree) {
                this.workplace[x].toWork = "inFix";
                this.workers[j].whichM = this.workplace[x].id;
                this.workers[j].isFree = false;
                break;
              }
            }
          }
        }
        for (var z = 0; z < this.workplace.length; z++) {
          if (
            this.time >= this.workplace[z].endFix &&
            this.workplace[z].toWork == "inFix"
          ) {
            var id = this.workplace[z].id; // id машины которую починили
            for (i = 0; i < this.workers.length; i++) {
              if (this.workers[i].whichM == id) {
                this.workers[i].isFree = true;
                this.workers[i].whichM = "";
                this.workplace[z].toWork = "inReserve";
                break;
              }
            }
          }
        }
        for (var y = 0; y < this.workplace.length; y++) {
          var workingMachine = this.workplace.filter(x => x.toWork == "inWork");
          if (workingMachine.length < this.enteredData.nowon) {
            if (this.workplace[y].toWork == "inReserve") {
              this.workplace[y].toWork = "inWork";
              this.workplace[y].timeWork =
                this.exDistribution(this.enteredData.avgTimeWork) + this.time;
              this.workplace[y].timeFix = this.exDistribution(this.enteredData.avgTimeFix);
            }
          }
        }
        //Высчитываем среднее кол-во простаивающих машин
        var stopingMachine = this.workplace.filter(x => x.toWork == "broke");
        this.stopingArr.push(stopingMachine.length);
        var sum = 0;
        for (i = 0; i < this.stopingArr.length; i++) {
          sum += this.stopingArr[i];
        }
        this.$refs.firstDT.desserts[0].avgStoping = parseFloat(
          sum / this.stopingArr.length
        ).toFixed(2);

        //Высчитываем среднее кол-во машин простаивающих в резерве
        var countMachine = this.workplace.filter(x => x.toWork == "inReserve");
        this.reserveArr.push(countMachine.length);
        var sum1 = 0;
        for (i = 0; i < this.reserveArr.length; i++) {
          sum1 += this.reserveArr[i];
        }
        this.$refs.firstDT.desserts[0].avgReserve = parseFloat(
          sum1 / this.reserveArr.length
        ).toFixed(2);

        //Высчитываем среднее кол-во работающих машин
        var countMachine1 = this.workplace.filter(x => x.toWork == "inWork");
        this.workingArr.push(countMachine1.length);
        var sum2 = 0;
        for (i = 0; i < this.workingArr.length; i++) {
          sum2 += this.workingArr[i];
        }
        this.$refs.firstDT.desserts[0].avgWorking = parseFloat(
          sum2 / this.workingArr.length
        ).toFixed(2);

        //Высчитываем среднее кол-во машин в ремонте
        var countMachine2 = this.workplace.filter(x => x.toWork == "inFix");
        this.fixingArr.push(countMachine2.length);
        var sum3 = 0;
        for (i = 0; i < this.fixingArr.length; i++) {
          sum3 += this.fixingArr[i];
        }
        this.$refs.firstDT.desserts[0].avgFixing = parseFloat(
          sum3 / this.fixingArr.length
        ).toFixed(2);

        //Расчитываем сколько денег потеряли из-за простоя рабочих мест
        var xx = this.workplace.filter(x => x.toWork == "inWork");

        if (xx.length < this.enteredData.nowon) {
          var yy = this.enteredData.nowon - xx.length; // Количство простаивающих рабочих мест. Это и есть сколько часов мы потеряли
          this.loseMoneyNotWorking += yy;
        }

        if (this.time % 40 == 0) {
          this.avgLoseMoneyNotWorking =
            (this.loseMoneyNotWorking / 8) * this.enteredData.costStopPerDay;
          this.avgLoseMoneyNotWorkingWeeks.push(this.avgLoseMoneyNotWorking);
          this.$refs.firstDT.desserts[0].avgLoseMoneyNotWorking = this.avgLoseMoneyNotWorking;
          this.loseMoneyNotWorking = 0;
        }

        this.time++;
      } else {
        clearInterval(this.interval);
        var pastDays = Math.floor((this.time - 1) / 8) //Сколько прошло дней
        this.salaryWorker = this.workers.length * pastDays * this.enteredData.costSalaryPerDay
        this.priceOfArend = this.countArendMachine * pastDays * this.enteredData.costArendPerDay
        var sum4 = 0
        for ( i = 0; i < this.avgLoseMoneyNotWorkingWeeks.length; i++) {
          sum4 = sum4 + this.avgLoseMoneyNotWorkingWeeks[i]
        }
        this.loseMoneyNotWorkingSum = sum4 / this.avgLoseMoneyNotWorkingWeeks.length
        this.loseMoneyNotWorkingSum1 = this.loseMoneyNotWorkingSum + 
                                      (this.salaryWorker / this.countWeeks) + 
                                      (this.priceOfArend / this.countWeeks)
        this.loseMoneyNotWorkingSum2 = sum4
        this.$refs.firstDT.loading = false
        this.dialog = true
        this.loseMoney = this.salaryWorker + this.priceOfArend + sum4
      }
    },
    run(value) {
      alert("Заполняем массивы")
      this.enteredData = value
      for (var i = 0; i < this.enteredData.nowon; i++) {
        var newMachine = {
          id: i,
          toWork: "inWork",
          timeWork: this.exDistribution(this.enteredData.avgTimeWork),
          timeFix: this.exDistribution(this.enteredData.avgTimeFix),
          startFix: "",
          endFix: ""
        }; 
        this.workplace.push(newMachine);
      }
      for (var j = i; j < parseInt(this.enteredData.spareMachine) + i; j++) {
        var newSpareMachine = {
          id: j,
          toWork: "inReserve",
          timeWork: this.exDistribution(this.enteredData.avgTimeWork),
          timeFix: this.exDistribution(this.enteredData.avgTimeFix)
        };
        this.spareplace.push(newSpareMachine);
        this.countArendMachine++;
      }
      for (var x = 0; x < this.enteredData.worker; x++) {
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
  },
  computed: {
    COMworkplaceWork: function() {
      return this.workplace.filter(function(u) {
        return u.toWork == "inWork";
      });
    },
    COMworkplaceBroke: function() {
      return this.workplace.filter(function(u) {
        return u.toWork == "broke";
      });
    },
    COMworkplaceFix: function() {
      return this.workplace.filter(function(u) {
        return u.toWork == "inFix";
      });
    },
    COMworkplaceReserve: function() {
      return this.workplace.filter(function(u) {
        return u.toWork == "inReserve";
      });
    }
  }
};
</script>

<style>
#workplace {
  border: 2px solid green;
  border-radius: 5px;
  width: 30px;
  height: 30px;
  margin: 25px;
}
h2 {
  text-align: center;
}
#spareplace {
  border: 2px solid orange;
  border-radius: 5px;
  width: 30px;
  height: 30px;
  margin: 25px;
}
#remont {
  border: 2px solid brown;
  border-radius: 5px;
  width: 150px;
  height: 50px;
  margin: 25px;
  padding: 10px;
}
#remontCeh {
  border: solid 1px grey;
}
#waitRemont {
  border: 2px solid black;
  border-radius: 5px;
  width: 30px;
  height: 30px;
  margin: 20px;
}
</style>
