<template>
  <div id="app">
    <table>
      <input placeholder="Filter" v-model="data.filter" />
      <tr>
        <th>State</th>
        <th>Positive</th>
        <th>Negative</th>
        <th>Recovered</th>
        <th>Total</th>
      </tr>
      <tr v-for="(info, i) in data.filteredStateInfo" :key="i">
        <td>{{ info.state }}</td>
        <td>{{ info.positive }}</td>
        <td>{{ info.negative }}</td>
        <td>{{ info.recovered ? info.recovered : 0 }}</td>
        <td>{{ info.total }}</td>
      </tr>
    </table>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, onMounted, computed } from "vue";
import axios from "axios";

export default defineComponent({
  name: "App",
  setup() {
    const data: any = reactive({
      stateInfo: [],
      filter: "",
      filteredStateInfo: computed(() =>
        data.stateInfo.filter((d: any) =>
          d.state.toLowerCase().includes(data.filter.toLowerCase())
        )
      ),
    });

    onMounted(async () => {
      const response = await axios(
        "https://api.covidtracking.com/v1/states/current.json"
      );
      data.stateInfo = response.data;
    });

    return {
      data,
    };
  },
});
</script>

<style lang="less">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  margin-bottom: 0;
  margin-left: auto;
  margin-right: auto;
  width: 80%;
}

table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>
