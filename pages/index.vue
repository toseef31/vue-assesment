<template>
  <div class="main-container-wrapper">
  <div class="row">
    <div class="col-9 m-auto">
      <div class="filter-section">
        <div class="row">
          <div class="col-12 col-md-4">
            <select class="form-control form-select">
            <option>Select Operator</option>
            <option v-for="operator in operators" :key="operator.id" :value="operator.id">
              {{ operator.operator }}
            </option>
            </select>
          </div>
          <div class="col-12 col-md-4">
            <select class="form-control form-select">
              <option>Select Game Type</option>
            </select>
          </div>
          <div class="col-12 col-md-4">
            <select class="form-control form-select">
              <option>Select Slate Name</option>
            </select>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="table-player-section">
    <div class="row flex-column-reverse flex-md-row">
      <div class="col-12 col-md-8">
        <div class="row">
          <div class="col-12 col-md-12">
            <div class="bg-white-10 rounded">
              <DataTable
                operator="SALAM"
                type="football"
                slate="14"
              />
            </div>
          </div>
        </div>
      </div>
      <div class="col-12 col-md-4">
        <div class="bg-white-10 rounded">
          <div class="player-image text-center pt-5">
            <img src="@/assets/Images/player1.png" class="mt-4" width="80%">
          </div>
          <div class="player-details pb-1 rounded bg-card-player">
            <div class="text-center player-name">John Doe</div>
            <div class="text-center player-rank">51</div>
            <div class="text-center player-subtitle pb-5 mb-5">Team</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import DataTable from '~/components/DataTable.vue';
export default {
  data() {
    return {
      page: 1,
      itemsPerPage: 8,
      headers: [
        { text: "Name", value: "name" },
        { text: "Team", value: "team" },
        { text: "Position", value: "position" },
        { text: "Salary", value: "salary", align: "end" },
        { text: "Points", value: "points", align: "end" },
      ],
      jsonData: [],
      operators:[]
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

        // If the operator isn't already in the Map, add it
        if (!operatorMap.has(operator)) {
          operatorMap.set(operator, operatorId);
        }
      }

      // Convert the Map to an array of objects
      this.operators = Array.from(operatorMap, ([operator, id]) => ({ operator, id }));
      console.log(this.operators);
    }
  },
  mounted() {
    this.loadData().then(() => {
      this.populateOperators();
    });
  }
}
</script>