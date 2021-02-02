<template>
  <div class="app">
    <header class="app-header">
      <ui5-shellbar primary-title="UI5 Web Components Vue Sample Application">
      </ui5-shellbar>
    </header>
    <ui5-card>
      <ui5-table class="demo-table" id="table">
        <!-- Columns -->
        <ui5-table-column slot="columns" popin-text="Name">
          <span style="line-height: 1.4rem">Name</span>
        </ui5-table-column>

        <ui5-table-column slot="columns" popin-text="Description">
          <span style="line-height: 1.4rem">Description</span>
        </ui5-table-column>

        <ui5-table-column slot="columns" popin-text="Category" demand-popin>
          <span style="line-height: 1.4rem">Category</span>
        </ui5-table-column>
        <ui5-table-column slot="columns" popin-text="Price" min-width="600">
          <span style="line-height: 1.4rem">Price</span>
        </ui5-table-column>
        <ui5-table-column slot="columns" popin-text="Button" demand-popin
          ><span style="line-height: 1.4rem"></span>
        </ui5-table-column>

        <ui5-table-row v-for="menuitem in all_menuitems" v-bind:key="menuitem">
          <ui5-table-cell>{{ menuitem.name }}</ui5-table-cell>
          <ui5-table-cell>{{ menuitem.description }}</ui5-table-cell>
          <ui5-table-cell>{{ menuitem.category }}</ui5-table-cell>
          <ui5-table-cell>
            <div v-for="menu in all_menus" v-bind:key="menu">
              <div
                v-if="menuitem.ID == menu.ID && menu.restaurant_ID == 1"
              >
                {{ menu.prijs }}
              </div>
            </div>
          </ui5-table-cell>
          <ui5-table-cell
            ><ui5-button design="Default" @click="openDialog(menuitem.ID, menuitem.name)"
              >Details</ui5-button
            ></ui5-table-cell
          >
        </ui5-table-row>
      </ui5-table>

      <ui5-dialog header-text="Alter price" ref="dialog">
        <div class="dialog-content">
          <div class="dialog-section">
            <ui5-label>Product name:</ui5-label>
            <ui5-label>{{ item }}</ui5-label>
          </div>

          <div class="dialog-section">
            <ui5-label>New price:</ui5-label>
            <ui5-input ref="priceInput"></ui5-input>
          </div>
        </div>

        <div slot="footer" class="dialog-footer">
          <ui5-button design="Emphasized" @click="submitNewPrice()"
            >OK</ui5-button
          >
          <ui5-button @click="closeDialog">Cancel</ui5-button>
        </div>
      </ui5-dialog>
    </ui5-card>
  </div>
</template>

<script>
import axios from "axios";
//import {VueCsvToggleHeaders, VueCsvSubmit, VueCsvMap, VueCsvInput, VueCsvErrors, VueCsvImport} from 'vue-csv-import';

import "@ui5/webcomponents-fiori/dist/ShellBar";
import "@ui5/webcomponents/dist/Table";
import "@ui5/webcomponents/dist/TableColumn";
import "@ui5/webcomponents/dist/TableRow";
import "@ui5/webcomponents/dist/TableCell";
import "@ui5/webcomponents/dist/Dialog";
import "@ui5/webcomponents/dist/Card";
import "@ui5/webcomponents/dist/Input";

export default {
  name: "app",
  data() {
    return {
      loading: false,
      item: null,
      priceInput: null,
      menu: null,
      menuitemID: null,
      all_menuitems: null,
      all_menus: null,
    };
  },
  methods: {
    openDialog(id, name) {
      this.item = name;
      this.menuitemID = id;
      //console.log(this.all_menus);
      this.$refs.dialog.open();
    },

    closeDialog() {
      this.$refs.dialog.close();
    },

    submitNewPrice() {
      //this is without database connection, POST does not work with CORS error
     /* for (var x in this.all_menus) {
        this.menu = this.all_menus[x];
        
        if (this.menuitemID == this.menu.menu_item_ID && this.menu.restaurant_ID == 1) {
          this.menu.price = this.$refs.priceInput.value;
          //console.log(this.menu.price);
          
        }
      }
      console.log(this.menu.price);*/
      
      this.closeDialog();
    },
  },
  mounted() {
    /* eslint-disable no-console */
    this.loading = true;
    axios
      .get(
        'https://f4132480trial-dev-horecahana-srv.cfapps.eu10.hana.ondemand.com/Owner/MenuItems'
        //"http://localhost:4004/Owner/MenuItems?$select=*"
      )
      .then((response) => (this.all_menuitems = response.data.value))
      .catch((error) => console.log(error))
      .finally(() => (this.loading = false));

    axios
      .get(
        'https://f4132480trial-dev-horecahana-srv.cfapps.eu10.hana.ondemand.com/Owner/Menu'
        //"http://localhost:4004/Owner/Menus?$select=*"
      )
      .then((response) => (this.all_menus = response.data.value))
      .catch((error) => console.log(error))
      .finally(() => (this.loading = false));
  },
};
</script>

<style>
body {
  margin: 0;
}

.app {
  height: 100%;
}

.header-toolbar {
  position: sticky;
  z-index: 42;
  background-color: rgba(255, 255, 255, 0.6);
  box-shadow: 0 4px 5px -5px #0a6ed1;
}

.ui5-logo {
  height: 2rem;
  margin-left: 2rem;
}

.app-title {
  margin-left: 1rem;
}

.app-content {
  height: calc(100% - 3rem);
  padding: 0 1rem;
  width: calc(100% - 2rem);
}

.create-todo-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  padding-top: 0.5rem;
  padding-bottom: 2rem;
  border-bottom: 1px solid #b3b3b3;
  box-sizing: border-box;
}

.add-todo-element-width {
  width: auto;
}

#add-input {
  flex: auto;
}

#date-picker {
  margin: 0 0.5rem 0 0.5rem;
}

.li-content {
  display: flex;
  width: 100%;
  justify-content: space-between;
  align-items: center;
}

.li-content-text {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.edit-btn {
  margin-right: 1rem;
}

.dialog-content {
  max-width: 320px;
  padding: 2rem 2rem;
}

.dialog-footer {
  display: flex;
  justify-content: flex-end;
  padding: 0.25rem 0.25rem 0 0.25rem;
  border-top: 1px solid #d9d9d9;
}

.title-textarea {
  height: 100px;
  display: inline-block;
  width: 100%;
}

.date-edit-fields {
  margin-top: 1rem;
}

@media (max-width: 600px) {
  .create-todo-wrapper {
    flex-direction: column;
  }

  .add-todo-element-width {
    width: 100%;
  }

  #date-picker {
    margin: 0.5rem 0 0.5rem 0;
  }
}
</style>
