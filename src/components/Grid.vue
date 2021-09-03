<template>
  <div id="app">
    <v-app id="inspire">
      <v-toolbar flat color="white">
        <v-toolbar-title>DATATABLE</v-toolbar-title>
        <v-spacer></v-spacer>

        <v-dialog v-model="dialog" max-width="500px">
          <v-btn slot="activator" color="primary" dark class="mb-2"
            >Yeni Kayıt</v-btn
          >
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container grid-list-md>
                <v-combobox
                  v-model="editedItem"
                  :items="desserts"
                  item-text="Name"
                  item-value="Type"
                  chips
                  label="arama yap.."
                ></v-combobox>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" flat @click.native="close"
                >İptal</v-btn
              >
              <v-btn color="blue darken-1" flat @click.native="save"
                >Kaydet</v-btn
              >
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>

      <v-tree-data-table
        :headers="headers"
        :items="desserts"
        :pagination.sync="pagination"
        :loading="loading"
        :total-items="pagination.totalItems"
        @load="load"
        hide-actions
        class="elevation-1"
      >
        <template slot="row" slot-scope="props">
          <td class="px-1">
            <v-list-tile-avatar
              color="primary"
              class="headline font-weight-light white--text"
            >
              {{
                props.item.Type.split(".")[
                  props.item.Type.split(".").length - 1
                ].charAt(0)
              }}
            </v-list-tile-avatar>
          </td>
          <td class="text-xs-left">{{ props.item.Name }}</td>
          <td class="justify-center layout px-0">
            <v-icon small class="mr-2" @click="editItem(props.item);"
              >edit</v-icon
            >
            <v-icon small @click="deleteItem(props.item);">delete</v-icon>
          </td>
        </template>

        <template slot="no-data">
          <v-btn color="primary" @click="initialize">Yenile</v-btn>
        </template>
      </v-tree-data-table>
    </v-app>
  </div>
</template>

<script>
const pageData = {
  loading: false,
  pagination: {
    // descending: boolean,
    // page: number,
    rowsPerPage: 10, // -1 for All
    // sortBy: string
    totalItems: 100
  },
  dialog: false,
  headers: [
    { text: "Type", value: "Type", align: "left", sortable: false },
    { text: "Name", value: "Name", sortable: false }
  ],
  desserts: [],
  editedIndex: -1,
  editedItem: null
};

window.pageData = pageData;

export default {
  data: () => {
    return pageData;
  },

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "Yeni Kayıt" : "Düzenleme";
    }
  },

  watch: {
    dialog(val) {
      val || this.close();
    }
  },

  created() {
    this.initialize();
  },

  methods: {
    editItem(item) {
      this.editedIndex = this.desserts.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      const index = this.desserts.indexOf(item);
      confirm("Silmek istediğinizden emin misiniz?") &&
        this.desserts.splice(index, 1);
    },

    close() {
      this.dialog = false;
      setTimeout(() => {
        this.editedItem = null;
        this.editedIndex = -1;
      }, 300);
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem);
      } else {
        this.desserts.push(this.editedItem);
      }
      this.close();
    },

    load() {
      this.desserts = [
        {
          id: "1",
          depth: 1,
          Type: "Intertech.Doqument.Product",
          Name: "Ürünler",
          children: [
            {
              id: "1.1",
              depth: 2,
              Type: "Intertech.Doqument.Product 1.1",
              Name: "Ürünler 1.1"
            }
          ]
        },
        {
          id: "2",
          depth: 1,
          Type: "Intertech.Doqument.Screen",
          Name: "Ekranlar"
        },
        {
          id: "3",
          depth: 1,
          Type: "Intertech.Doqument.Module",
          Name: "Modüller"
        }
      ];
    }
  }
};
</script>
