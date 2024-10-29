<template>
  <div class="main-container-wrapper">
  <div class="row">
    <div class="col-9 m-auto">
      <div class="filter-section">
        <div class="row">
          <div class="col-12 col-md-4">
            <select class="form-control form-select" @change="selectOperator">
            <option disabled selected>Select Operator</option>
            <option v-for="operator in operators" :key="operator.id" :value="operator.operator">
              {{ operator.operator }}
            </option>
            </select>
          </div>
          <div class="col-12 col-md-4">
            <select class="form-control form-select" @change="selectOperatorName">
              <option disabled selected>Select Game Type</option>
              <option v-for="operatorname in operatorNames" :key="operatorname.name" :value="operatorname.name">
                {{ operatorname.name }}
              </option>
            </select>
          </div>
          <div class="col-12 col-md-4">
            <select class="form-control form-select" @change="selectGame">
              <option disabled selected>Select Slate Name</option>
              <option v-for="game in games" :key="game.type" :value="game.type">
                {{ game.type }}
              </option>
            </select>
          </div>
        </div>
      </div>
    </div>
  </div>
  <DataTable :players="players" />
</div>
</template>

<script>
import DataTable from '~/components/DataTable.vue';
export default {
  data() {
    return {
      jsonData: [],
      operators:[],
      operatorNames:[],
      games:[],
      slates:[],
      selectedOperator:null,
      selectedOperatorName:null,
      selectedGame:null,
      selectedSlate:null,
      players:null,
    };
  },
  name: 'IndexPage',
  components: {
    DataTable
  },
  methods:{
    async loadData(){
      try {
        const response = await fetch('/data.json'); // Adjust the path based on the file location
        if (!response.ok) {
          throw new Error('Failed to load JSON file');
        }
        this.jsonData = await response.json();
      } catch (error) {
        console.error(error);
      }
    },
    populateOperators() {
      const operatorMap = new Map();

      for (let i = 0; i < this.jsonData.length; i++) {
        const operator = this.jsonData[i].operator;
        const operatorId = this.jsonData[i]._id;

        // Combine operator and operatorId to create a unique key
        const uniqueKey = `${operator}`;

        // If this uniqueKey isn't already in the Map, add the object
        if (!operatorMap.has(uniqueKey)) {
          operatorMap.set(uniqueKey, { id: operatorId, operator: operator });
        }
      }

      // Optional: Convert the Map to an array if needed
      this.operators = Array.from(operatorMap.values());

      console.log(this.operators);
    },


    selectOperator(event){
      this.selectedOperator = event.target.value;
      this.listOperatorName();
      this.games = null;
      this.listPlayers();
    },
    selectGame(event){
      this.selectedGame = event.target.value;
      this.listPlayers();
    },

    listOperatorName(){
      const operatorsNameSet = new Set();
      for(let i = 0 ; i < this.jsonData.length; i++){
        if(this.selectedOperator === this.jsonData[i].operator){
          const name = this.jsonData[i].operatorName;
          operatorsNameSet.add(name);
        }
      }
      this.operatorNames = Array.from(operatorsNameSet).map(name => ({name}));
    },
    listGameTypes() {
      const gameTypesSet = new Set(); // Use a Set to store unique game types

      for (let i = 0; i < this.jsonData.length; i++) {
        if(this.selectedOperator === this.jsonData[i].operator  && this.selectedOperatorName === this.jsonData[i].operatorName){

          const gameType = this.jsonData[i].operatorGameType;

          // Add the game type to the Set for uniqueness
          gameTypesSet.add(gameType);
        }
      }

      // Convert the Set to an array of objects
      this.games = Array.from(gameTypesSet).map(type => ({ type }));
    },
    selectOperatorName(event){
      this.selectedOperatorName = event.target.value;
      this.listGameTypes();
      this.listPlayers();
    },
    listSlates(){
      const slateTypesSet = new Set();
      for(let i = 0 ; i < this.jsonData.length ; i++){
        const slate = this.jsonData[i].slateId;
        slateTypesSet.add(slate);
      }
      console.log(slateTypesSet);
      this.slates = Array.from(slateTypesSet).map(type => ({type}))
    },
    listPlayers() {
  let players = [];
  if (this.selectedOperator === null) {
    this.players = null;
    return;
  }

  for (let i = 0; i < this.jsonData.length; i++) {
    const data = this.jsonData[i];

    const operatorMatch = this.selectedOperator === data.operator;
    const operatorNameMatch = this.selectedOperatorName === data.operatorName || this.selectedOperatorName === null;
    const gameMatch = this.selectedGame === data.operatorGameType || this.selectedGame === null;

    if (operatorMatch && operatorNameMatch && gameMatch) {
      players = players.concat(data.dfsSlatePlayers); // Flatten the array
    }
  }

  this.players = players;
  console.log(this.players);
}


  },

  mounted() {
    this.loadData().then(() => {
      this.populateOperators();
    });
  }
}
</script>