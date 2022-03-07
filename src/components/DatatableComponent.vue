<template>
  <div class="">
    <h1 style="text-align: center;">Datatable with 3rd Party API</h1>
    <v-data-table
      :page="page"
      :pageCount="numberOfPages"
      :headers="headers"
      :items="port"
      :options.sync="options"
      :server-items-length="totalPorts"
      :loading="loading"
      class="elevation-1"
    >
    </v-data-table>
  </div>
</template>
<style scoped></style>
<script>
import axios from "axios";
export default {
  name: "DatatableComponent",
  data() {
    return {
      page: 1,
      totalPorts: 0,
      numberOfPages: 0,
      port: [],
      loading: true,
      options: {},
      headers: [
        { text: "ID", value: "id" },
        { text: "Name", value: "name" },
        { text: "Country", value: "country" },
        { text: "Continent", value: "continent" },
        { text: "Coordinates", value: "coordinates" },
      ],
    };
  },
  watch: {
    options: {
      handler() {
        this.readDataFromAPI();
      },
    },
    deep: true,
  },
  methods: {
    readDataFromAPI() {
      this.loading = true;
      const { page, itemsPerPage } = this.options;
      console.log("Page Number ", page, itemsPerPage);
      let pageNumber = page - 1;
      axios
        .get(
          "http://apitest.cargofive.com/api/ports?page=" +
            itemsPerPage +
            "&page=" +
            pageNumber
        )
        .then((response) => {
          this.loading = false;
          this.port = response.data.data;
          this.totalPorts = response.data.totalPassengers;
          this.numberOfPages = response.data.meta.total;
        });
    },
  },
  mounted() {
    this.readDataFromAPI();
  },
};
</script>
