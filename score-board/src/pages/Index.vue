<template>
  <q-page padding>
    <!-- <score-board> </score-board> -->

  <div class="container">

    <div>
      {{ columns }}
      {{ rows }}
    </div>
    <h4>Scoreboard</h4>
      <div class="q-gutter-sm" v-if="playerNumberSelected == false">
        <p>Please select player number</p>
        <q-radio v-model="number" val="1" label="1" color="teal" />
        <q-radio v-model="number" val="2" label="2" color="orange" />
        <q-radio v-model="number" val="3" label="3" color="red" />
        <q-radio v-model="number" val="4" label="4" color="cyan" />

        <p>Your selection is: <strong>{{ number }}</strong></p>
        <button type="button" @click="choosePlayer">
          Choose
        </button>
      </div>

    <div class="q-pa-md"  v-else-if="numberOfRoundsSelected == false">
      <div class="q-px-sm q-mt-sm"  >
        <p>Please select number of Rounds</p>
      </div>
        <q-badge color="secondary">
          Number of Games: {{ value }} (1 to 15)
        </q-badge>

        <q-slider
          v-model="value"
          :min="1"
          :max="15"
          :step="1"
          snap
          label
          color="purple"
        />
        <button type="button" @click="chooseRound">
          Select Round
        </button>
    </div>

    <div class="q-pa-md" v-if="numberOfRoundsSelected == true && numberOfRoundsSelected== true && playerNames.length != number">
    <input v-model="text" placeholder="Add Player names" >
    <span>Player names: {{ playerNames }}</span>
    <button type="button" @click="addPlayer">
      Add
    </button>
    <!-- <div v-if="playerNames.length == number">
      <button type="button" @click="createTable">
        Create Table
      </button>
     </div> -->
    </div>

<div class="q-pa-md" v-if="columns.length > 2">
    <q-table
      title="Score Board"
      :rows="rows"
      :columns="columns"
      row-key="name"
      binary-state-sort
      :dense="$q.screen.lt.md"
      virtual-scroll
      :rows-per-page-options="[0]"
    >
      <template v-slot:body="props">
        <q-tr :props="props">
          <!-- <q-td key="name" :props="props">
            {{ props.row.name }}
            <q-popup-edit v-model="props.row.name">
              <q-input v-model="props.row.name" dense autofocus counter />
            </q-popup-edit>
          </q-td> -->

          <!-- <q-td v-for="(entry, i) in columns.name" :key="i" :props="props">{{ props.row.entry }}</q-td> -->
        <q-td
          class="td-my"
          v-for="(col, i) in props"
          :key="i"
          :props="props"
          >{{ props.col.value }}

            <q-popup-edit v-model="props.row.name" title="Update Score" buttons>
              <q-input v-model="props.row.name" dense autofocus counter />
            </q-popup-edit>
        </q-td>
          <!-- <q-td key="name" :props="props">
            {{ props.columns.name }}
            <q-popup-edit v-model="props.columns.name" title="Update calories" buttons>
              <q-input type="number" v-model="props.columns.name" dense autofocus />
            </q-popup-edit>
          </q-td> -->
          <!-- <q-td key="fat" :props="props">
            <div class="text-pre-wrap">{{ props.row.fat }}</div>
            <q-popup-edit v-model="props.row.fat">
              <q-input type="textarea" v-model="props.row.fat" dense autofocus />
            </q-popup-edit>
          </q-td> -->
          <!-- <q-td key="carbs" :props="props">
            {{ props.row.carbs }}
            <q-popup-edit v-model="props.row.carbs" title="Update carbs" buttons persistent>
              <q-input type="number" v-model="props.row.carbs" dense autofocus hint="Use buttons to close" />
            </q-popup-edit>
          </q-td> -->
            <!-- <q-td v-for="(entry, i) in numberOfGames" :key="i">
              {{ entry }}
            </q-td> -->
          <!-- <q-td key="protein" :props="props">{{ props.row.protein }}</q-td>
          <q-td key="sodium" :props="props">{{ props.row.sodium }}</q-td>
          <q-td key="calcium" :props="props">{{ props.row.calcium }}</q-td>
          <q-td key="iron" :props="props">{{ props.row.iron }}</q-td> -->
        </q-tr>
      </template>
    </q-table>
  </div>

  </div>

  </q-page>
</template>

<script>
/* eslint-disable */
import { defineComponent, ref } from 'vue'
export default defineComponent({
  name: 'PageIndex',
  components: {
    // ScoreBoard: () => import('components/ScoreBoard')
  },
  data: () => ({
edit: null,
      employees: [{
          id: 0,
          employeeName: "Jane",
          joinDate: "11-11-1111",
          selectedDepartment: "IT",
          jobDescription: "Nerd"
        },
        {
          id: 1,
          employeeName: "Peter",
          joinDate: "12-12-1212",
          selectedDepartment: "Accounting",
          jobDescription: "Moneier"
        }
      ],
      fields: [{
          key: 'employeeName',
          label: 'Employee Name',
          sortable: true
        },
        {
          key: 'joinDate',
          label: 'Join Date',
          sortable: true
        },
        {
          key: 'selectedDepartment',
          label: 'Selected Department',
          sortable: true
        },
        {
          key: 'jobDescription',
          label: 'Job Description',
          sortable: true
        },
        {
          key: 'actions',
          label: 'Actions'
        }
      ],
    columns: [
      {
        name: 'name',
        required: true,
        label: 'Number of Plays',
        align: 'left',
        field: row => row.name,
        format: val => `${val}`,
        sortable: true
      }
    ],
    rows: [

    ],
    number: ref('2'),
    playerNumberSelected: false,
    numberOfRoundsSelected: false,
    name: '',
    text: '',
    score: '',
    playerNumber: null,
    allScores: [],
    playerNames: ['ooo'],
    numberOfGames: [],
    value: ref(3)
  }),
  computed: {
    rows() {
      return this.employees.length
    },
    sortedList: function () {
      return this.allScores.slice().sort(function (a, b) {
        return b.score - a.score
      })
    },
    sortedList2: function () {
      return this.playerNames.slice().sort(function (a, b) {
        return b.score - a.score
      })
    }
  },
  methods: {
    onEdit(id) {
      this.edit = this.edit !== id ? id : null;
    },
    choosePlayer () {
      // console.log("choosed", this.number, this.value)
      this.playerNumber = this.number
      // this.value = this.value
      this.playerNumberSelected = true
    },
    addPlayer () {
      this.playerNames.push(this.text)
      // console.log(this.playerNames.length, this.playerNumber)
      // if (this.playerNames.length == this.playerNumber) {
      // this.columns.push({ name: this.text, align: 'left',
      //                       label: this.text, field: this.text, sortable: true,
      //                       // field: row => row.number,
      //                       // format: val => `${val}`
      //                       })
      // console.log("columns:", this.columns)
      console.log(this.playerNames)
      if (this.playerNames.length == this.number) {
        this.createTable()
      }
      // this.createTable(this.text)
      // }
    },
    chooseRound () {
      this.numberOfRoundsSelected = true
      this.numberOfGames = this.createListFromArray(this.numberOfGames, this.value)
      console.log('number of games:', this.value)
      console.log('number of games list:', this.numberOfGames)
    },
    createListFromArray(array, length) {
      var new_array = new Array(length);
      for (var i = 0; i < new_array.length; i++) {
        new_array[i] = "x"
        }
      for (var i = 0; i < array.length; i++) {
        new_array[array[i]] = array[i]
        }
        // this.rows.push({ name: this.text, label: this.text, field: this.text})
        // this.array == new_array
        // console.log(new_array)
        return new_array
        // console.log(createArrayFromArray(arr, 7)); //[null, null, 2, null, 4, null, 6]
    },
    createTable () {
      console.log('simdi tabloyu yarat')
      for (const i in this.playerNames) {
        this.columns.push({
                            name: this.playerNames[i],
                            required: true,
                            align: 'left',
                            label: this.playerNames[i],
                            field: this.playerNames[i],
                            sortable: true,
                            // field: row => row.name,
                            // format: val => `${val}`,
                          })
        // console.log(this.playerNames[i])
      }
      console.log("columns", this.columns)

      for(var i=0; i<this.numberOfGames.length; i++){
          var obj = {};

          for(var j=0; j<=this.numberOfGames[i].length; j++){
              obj["name"] = i + 1
              obj[this.playerNames[j]] = i;

            }
          this.rows.push(obj);
      }
      console.log("rows", this.rows)
    },
    onSubmit () {
      this.allScores.push({ name: this.name, score: this.score })
      this.clearForm()
    },
    clearForm () {
      this.name = ''
      this.score = ''
    }
  }
})
</script>
