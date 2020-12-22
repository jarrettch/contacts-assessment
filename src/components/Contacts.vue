<template>
  <v-container>
    <v-row class="text-center">

      <v-col class="mb-4">
        <v-data-table :items="contacts" :headers="headers" class="elevation-1">
          
          <template v-slot:top>
            <v-toolbar flat>
              <v-toolbar-title>Contacts</v-toolbar-title>
              <v-divider class="mx-4" inset vertical></v-divider>
              <v-spacer></v-spacer>
              <v-dialog v-model="dialog" max-width="500px">

                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    color="primary"
                    dark
                    class="mb-2"
                    v-bind="attrs"
                    v-on="on"
                  >
                    New Item
                  </v-btn>
                </template>

                <v-card>
                  <v-card-title>
                    <span class="headline">{{ formTitle }}</span>
                  </v-card-title>

                  <v-card-text>
                    <v-container>
                      <v-row>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            v-model="editedItem.email"
                            label="Email Address"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            v-model="editedItem.name"
                            label="Full Name"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            v-model="editedItem.address"
                            label="Address"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            v-model="editedItem.city"
                            label="City"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            v-model="editedItem.state"
                            label="State"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            v-model="editedItem.zipcode"
                            label="Postal Code"
                          ></v-text-field>
                        </v-col>
                      </v-row>
                    </v-container>
                  </v-card-text>

                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="close">
                      Cancel
                    </v-btn>
                    <v-btn color="blue darken-1" text @click="save">
                      Save
                    </v-btn>
                  </v-card-actions>

                </v-card>
              </v-dialog>

              <v-dialog v-model="dialogDelete" max-width="500px">
                <v-card>
                  <v-card-title class="headline">Are you sure you want to delete this contact?</v-card-title>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
                    <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
                    <v-spacer></v-spacer>
                  </v-card-actions>
                </v-card>
              </v-dialog>
              
            </v-toolbar>
          </template>

          <template v-slot:[`item.actions`]="{ item }">
            <v-icon
              small
              class="mr-2"
              @click="editItem(item)"
            >
              mdi-pencil
            </v-icon>
            <v-icon
              small
              @click="deleteItem(item)"
            >
              mdi-delete
            </v-icon>
          </template>
          <template v-slot:no-data>
            <v-btn
              color="primary"
              @click="initialize"
            >
              Reset
            </v-btn>
          </template>
        </v-data-table>
      </v-col>

    </v-row>
  </v-container>
</template>

<script>
import contacts from "../../contacts.json";
export default {
  name: "Contacts",
  data: () => ({
    dialog: false,
    dialogDelete: false,
    contacts: contacts,
    editedIndex: -1,
    editedItem: {
      name: "",
      email: "",
      address: "",
      city: "",
      state: "",
      zipcode: "",
    },
    defaultItem: {
      name: "",
      email: "",
      address: "",
      city: "",
      state: "",
    },
    headers: [
      { text: "Name", value: "name" },
      { text: "Email", value: "email" },
      { text: "Address", value: "address" },
      { text: "City", value: "city" },
      { text: "State", value: "state" },
      { text: "Postal Code", value: "zipcode" },
      { text: 'Actions', value: 'actions', sortable: false },
    ],
  }),
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Contact" : "Edit Contact";
    },
  },
  methods: {
    editItem(item) {
      this.editedIndex = this.contacts.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.editedIndex = this.contacts.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.contacts.splice(this.editedIndex, 1);
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
        Object.assign(this.contacts[this.editedIndex], this.editedItem);
      } else {
        this.contacts.push(this.editedItem);
      }
      this.close();
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
};
</script>
