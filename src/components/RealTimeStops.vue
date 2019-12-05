<template>
  <div class="hello">
    <v-container>
      <v-layout row wrap class="pt-5" justify-center>
        <v-flex sm6>
          <v-text-field
            name="input-1"
            label="Enter Bus Stop Number (example 2236)"
            type="number"
            autofocus
            outline
            v-model="stopid"
            class="text-xs-center text-black"
            @keyup.enter="onSubmit()"
          ></v-text-field>
        </v-flex>
        <v-flex sm3>
          <v-btn @click="onSubmit()" color="success"
            ><v-icon sharp class="pr-2"> search </v-icon>search
          </v-btn>
        </v-flex>
        <br />
      </v-layout>
      <v-layout>
        <v-flex>
          <v-data-table
            :headers="headers"
            :items="items"
            :loading="loading"
            class="elevation-4"
            hide-default-footer
            v-if="submitted"
          >
            <v-progress-linear
              slot="progress"
              height="4"
              color="amber lighten-1"
              indeterminate
            ></v-progress-linear>
            <template slot="items" slot-scope="results">
              <td>{{ results.item.route }}</td>
              <td class="text-sm-center">
                {{ results.item.destination }} via {{ results.item.origin }}
              </td>
              <td class="text-sm-center hidden-xs-only">
                {{ results.item.arrivaldatetime }}
              </td>
              <td class="text-sm-center">{{ results.item.duetime }} min</td>
            </template>
            <template slot="no-data">
              <v-alert :value="true" color="warning" icon="warning">
                no results
              </v-alert>
            </template>
          </v-data-table>
        </v-flex>
      </v-layout>
      <v-layout>
        <v-flex>
          <br />
          <h4 v-if="submitted">
            Last updated {{ timestamp }}
            <v-icon @click="onSubmit()" color="success">
              refresh
            </v-icon>
          </h4>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "RealTimeStops",
  data() {
    return {
      due: false,
      stopidInput: "",
      stopid: "",
      submitted: false,
      numberofresults: "",
      response: "",
      loading: true,
      timestamp: "",
      headers: [
        {
          text: "Route",
          align: "center",
          sortable: false,
          value: "route",
          class: ["text-sm-center"]
        },
        {
          text: "Destination",
          value: "destination",
          sortable: false,
          align: "center",
          class: ["text-sm-center"]
        },
        {
          text: "Expected Time",
          value: "arrivaldatetime",
          sortable: true,
          class: ["hidden-xs-only", "text-sm-center"]
        },
        {
          text: "Due Time",
          value: "duetime",
          class: ["text-sm-center"],
          align: "center"
        }
      ],
      items: [],
      results: []
    };
  },
  methods: {
    async onSubmit() {
      this.loading = true;
      this.submitted = true;
      let url = `https://cors-anywhere.herokuapp.com/https://data.dublinked.ie/cgi-bin/rtpi/realtimebusinformation?stopid=${this.stopid}`;
      try {
        const response = await axios.get(url);
        this.items = response.data.results;
        this.timestamp = response.data.timestamp;
        this.numberofresults = response.data.numberofresults;
        this.stopid = response.data.stopid;
        this.items = response.data.results;
        this.loading = false;
      } catch (error) {
        console.error(error);
        this.loading = false;
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only 1 -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
