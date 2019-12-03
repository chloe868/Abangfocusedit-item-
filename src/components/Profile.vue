<template>
  <div>
    <Header />
    <br /><br /><br /><br />
    <v-card class="mx-auto" max-width="500" max-height="600">
      <v-img
        src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg"
        height="200px"
      ></v-img>
      <center>
        <input type="file" name="file" ref="myFileRef" multiple accept="/*image" @change="beforeUpload"/>
        <v-button type="button" v-on:click="uploadFile()">Upload</v-button>
      </center>
      <v-card-title>Catalina Car Rental</v-card-title>
      <v-card-subtitle>One of the Best Car Rental in the Philippines</v-card-subtitle>

      <v-card-actions>
        <v-btn text>Share</v-btn>
        <v-btn color="purple" text>Explore</v-btn>
        <v-spacer></v-spacer>
        <v-btn icon @click="show = !show">
          <v-icon>{{ show ? "mdi-chevron-up" : "mdi-chevron-down" }}</v-icon>
        </v-btn>
      </v-card-actions>

      <v-expand-transition>
        <div v-show="show">
          <v-divider></v-divider>
          <v-card-text>
            I'm a thing. But, like most politicians, he promised more than he
            could deliver. You won't have time for sleeping, soldier, not with
            all the bed making you'll be doing. Then we'll go with that data
            file! Hey, you add a one and two zeros to that or we walk! You're
            going to do his laundry? I've got to find a way to escape.
          </v-card-text>
          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ on }">
              <center>
                <v-btn color="primary" dark class="mb-2" v-on="on"
                  >Update profile</v-btn>
              </center>
            </template>
            <v-card>
              <v-card-title>
                <span class="headline">{{ formTitle }}</span>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field v-model="editedItem.name" label="Company Name"></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field v-model="editedItem.brand" label="Brand"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.sit"
                        label="Sitting Capacity"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.loc"
                        label="Location"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.rate"
                        label="Rate"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
                <v-btn color="blue darken-1" text @click="save">Save</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </div>
      </v-expand-transition>
    </v-card>
  </div>
</template>
<script>
import Header from "..//components/Header.vue";
export default {
  name: "fileupload",
  components: {
    Header
  },
  data() {
    return {
      file: " ",
      show: false,
      dialog: false,
      vehicle: [],
      editedIndex: -1,
      editedItem: {
        name: "",
        brand: "",
        sit: 0,
        loc: "",
        rate: ""
      },
      defaultItem: {
        name: "",
        brand: "",
        sit: 0,
        loc: "",
        rate: ""
      }
    };
  },
  methods: {
    close() {
      this.dialog = false;
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      }, 300);
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.vehicle[this.editedIndex], this.editedItem);
      } else {
        this.vehicle.push(this.editedItem);
      }
      this.close();
    },
    beforeUpload() {
      this.file = this.$refs.myFileRef.files;
    },
    uploadFile: function() {
      let formData = new FormData();

      formData.append("img", this.file);
      for (let i = 0; i < this.file.length; i++) {
        formData.append("img", this.file[i]);
      }
      this.$axios
        .post(`http://localhost:5000/uploadMultiple`, formData)
        .then(res => {
          console.log(res);
          alert(res);
        })
        .catch(error => {
          console.error("file upload failed", error);
        });
    }
  },
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    }
  },

  watch: {
    dialog(val) {
      val || this.close();
    }
  }
};
</script>
<style scoped></style>
