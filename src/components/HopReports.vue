<template>
  <v-container class="fill-height">
    <v-responsive class="fill-height py-8">
      <h1 class="text-h2 font-weight-bold align-center text-center">
        Hop Reports
      </h1>
      <div class="py-8" />
      <v-row class="d-flex align-center justify-center">
        <v-col cols="10 ">
          <v-data-table
            :headers="headers"
            :items="hopReports"
            :sort-by="[{ key: 'actual_physical_attendance', order: 'asc' }]"
            class="elevation-2"
          >
            <template v-slot:top>
              <v-toolbar flat>
                <v-divider class="mx-4" inset vertical></v-divider>
                <v-spacer></v-spacer>
                <v-dialog v-model="dialog" max-width="500px">
                  <template v-slot:activator="{ props }">
                    <v-btn color="primary" dark class="mb-2" v-bind="props">
                      New Item
                    </v-btn>
                  </template>
                  <v-card>
                    <v-card-title>
                      <span class="text-h5">{{ formTitle }}</span>
                    </v-card-title>

                    <v-card-text>
                      <v-container>
                        <v-row>
                          <v-col cols="12" sm="6">
                            <v-text-field
                              v-model="editedItem.expected_physical_attendance"
                              label="Expected Physical Attendance"
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12" sm="6">
                            <v-text-field
                              v-model="editedItem.actual_physical_attendance"
                              label="Actual Physical Attendance"
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12" sm="6">
                            <v-text-field
                              v-model="editedItem.expected_online_attendance"
                              label="Expected Online Attendance"
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12" sm="6">
                            <v-text-field
                              v-model="editedItem.actual_online_attendance"
                              label="Actual Online Attendance"
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12" sm="6">
                            <v-text-field
                              v-model="editedItem.expected_first_timers"
                              label="Expected Firsttimers"
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12" sm="6">
                            <v-text-field
                              v-model="editedItem.actual_first_timers"
                              label="Actual Firsttimers"
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12" sm="6">
                            <v-text-field
                              v-model="editedItem.souls_saved"
                              label="Souls Saved"
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12" sm="6">
                            <v-text-field
                              v-model="editedItem.hop_date"
                              label="Hop Date"
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12">
                            <v-text-field
                              v-model="editedItem.hop_review"
                              label="Hop Review"
                            ></v-text-field>
                          </v-col>
                        </v-row>
                      </v-container>
                    </v-card-text>

                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn
                        color="blue-darken-1"
                        variant="text"
                        @click="close"
                      >
                        Cancel
                      </v-btn>
                      <v-btn color="blue-darken-1" variant="text" @click="save">
                        Save
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
                <v-dialog v-model="dialogDelete" max-width="500px">
                  <v-card>
                    <v-card-title class="text-h5"
                      >Are you sure you want to delete this item?</v-card-title
                    >
                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn
                        color="blue-darken-1"
                        variant="text"
                        @click="closeDelete"
                        >Cancel</v-btn
                      >
                      <v-btn
                        color="blue-darken-1"
                        variant="text"
                        @click="deleteItemConfirm"
                        >OK</v-btn
                      >
                      <v-spacer></v-spacer>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
              </v-toolbar>
            </template>
            <template v-slot:item.actions="{ item }">
              <v-icon size="small" class="me-2" @click="editItem(item.raw)">
                mdi-pencil
              </v-icon>
              <v-icon size="small" @click="deleteItem(item.raw)">
                mdi-delete
              </v-icon>
            </template>
            <template v-slot:no-data>
              <v-btn color="primary" @click="initialize"> Reset </v-btn>
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
  data: () => ({
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        title: "Expected Physical Attendance",
        align: "start",
        sortable: true,
        key: "expected_physical_attendance",
      },
      {
        title: "Actual Physical Attendance",
        key: "actual_physical_attendance",
      },
      {
        title: "Expected Online Attendance",
        key: "expected_online_attendance",
      },
      { title: "Actual Online Attendance", key: "actual_online_attendance" },
      { title: "Expected First Timers", key: "expected_first_timers" },
      { title: "Actual First Timers", key: "actual_first_timers" },
      { title: "Souls Saved", key: "souls_saved" },
      { title: "Hop Date", key: "hop_date" },
      { title: "Actions", key: "actions", sortable: false },
    ],
    hopReports: [],
    editedIndex: -1,
    editedItem: {
      expected_physical_attendance: "",
      actual_physical_attendance: "",
      expected_online_attendance: "",
      actual_online_attendance: "",
      expected_first_timers: "",
      actual_first_timers: "",
      souls_saved: "",
      hop_date: "",
      hop_review: "",
    },
    defaultItem: {
      expected_physical_attendance: "",
      actual_physical_attendance: "",
      expected_online_attendance: "",
      actual_online_attendance: "",
      expected_first_timers: "",
      actual_first_timers: "",
      souls_saved: "",
      hop_date: "",
    },
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    },
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  created() {
    this.initialize();
  },

  methods: {
    initialize() {
      axios
        .get("http://127.0.0.1:8000/api/v1/hopreport/?offset=0&limit=100")
        .then((response) => {
          this.hopReports = response.data;
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
        });
    },

    editItem(item) {
      this.editedIndex = this.hopReports.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.editedIndex = this.hopReports.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.hopReports.splice(this.editedIndex, 1);
      this.closeDelete();
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.hopReports[this.editedIndex], this.editedItem);
      } else {
        this.hopReports.push(this.editedItem);
      }
      this.close();
    },
  },
};
</script>
