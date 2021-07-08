<template>
  <q-page padding>
    <p>Test Page</p>

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

      <table>
          <th v-for="(name, id) in users" :key="id">{{ name.name }}</th>
          <!-- <th>{{ name}}</th> --> -->
          <!-- <th>Name</th>
          <th>Email</th> -->
          <th></th>
          <th></th>
        <tbody>
          <tr v-for="user in users" :class="{editing: user == editedUser}" :key="user.id" v-cloak>
            <td>
              <div class="view">
                {{user.name}}
              </div>
              <div class="edit">
                <input type="text" v-model="user.name"/>
              </div>
            </td>
            <td>
              <div class="view">
                {{user.email}}
              </div>
              <div class="edit">
                <input type="text" v-model="user.email"/>
              </div>
            </td>
            <td>
              <div class="view">
                <button @click="editData(user)">edit</button>
              </div>
              <div class="edit">
                <button @click="saveData(user)">save</button>
              </div>
            </td>
            <td><button>delete</button></td>
          </tr>
        </tbody>
      </table>
  </q-page>
</template>

<script>
import { defineComponent, ref } from 'vue'
/* eslint-disable */
export default defineComponent({
  name: 'PageIndex',
  components: {
    // ScoreBoard: () => import('components/ScoreBoard')
  },
  data: () => ({
    editMode: false,
    users: [
      { name: 'mimi', email: 'wewe@s.com'},
      { name: 'sam', email: 'some@mk.com'},
      { name: 'kev', email: 'wewe'}
    ],
    editedUser: null,
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
    value: ref(3),
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
  }),
  computed: {
    editableFields() {
      return this.fields.filter(field => field.editable)
    }
  },
  methods: {
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
    editData (user) {
      this.beforEditCache = user
      this.editedUser = user
    }
  }
})
</script>


<style scoped>
[v-cloak] {
      display: none;
    }
    .edit {
      display: none;
    }
    .editing .edit {
      display: block
    }
    .editing .view {
      display: none;
    }
</style>
