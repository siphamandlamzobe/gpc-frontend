<template>
  <v-container class="fill-height">
    <v-responsive class="fill-height py-8">
      <h1 class="text-h2 font-weight-bold align-center text-center">
        Service Report
      </h1>
      <div class="py-8" />

      <v-row class="d-flex">
        <v-col cols="10 mx-auto">
          <div class="text-right align-end bg-red-2">
            <v-btn prepend-icon="mdi-plus-circle">
              <template v-slot:prepend>
                <v-icon color="success"></v-icon>
              </template>

              Add New Report
            </v-btn>
          </div>
        </v-col>
      </v-row>
      <v-row class="d-flex align-center justify-center">
        <v-col cols="10">
          <v-data-table
            v-model:expanded="expanded"
            :headers="serviceReportHeaders"
            :items="serviceReports"
            item-value="attendance"
            show-expand
            class="elevation-2"
          >
            <template v-slot:top>
              <v-toolbar flat>
                <v-toolbar-title>Service Reports</v-toolbar-title>
              </v-toolbar>
            </template>
            <template v-slot:expanded-row="{ columns, item }">
              <tr>
                <td :colspan="columns.length">
                  {{ item.raw.service_review }}
                </td>
              </tr>
            </template>
          </v-data-table>
        </v-col>
      </v-row>
    </v-responsive>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      expanded: [],
      serviceReportHeaders: [
        {
          title: "Attendance",
          align: "start",
          sortable: true,
          key: "attendance",
        },
        { title: "First Timers", key: "first_timers" },
        { title: "Souls Saved", key: "souls_saved" },
        { title: "Service Date", key: "service_date" },
        { title: "", key: "data-table-expand" },
      ],
      serviceReports: [],
    };
  },
  mounted() {
    this.getServiceReports();
  },
  methods: {
    getServiceReports() {
      axios
        .get("http://127.0.0.1:8000/api/v1/servicereport/?offset=0&limit=100")
        .then((response) => {
          this.serviceReports = response.data;
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
        });
    },
  },
};
</script>
