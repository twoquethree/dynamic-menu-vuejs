<template>
    <v-dialog
        v-model="dialog"
        persistent
        scrollable
        max-width="300px">
        <v-card>
            <template v-if="title.parent_id" >
                <v-card-title class="subheading">
                    <v-btn
                    icon
                    flat
                    @click="returnToParentItem(title.parent_id)">
                        <v-icon>
                        chevron_left
                        </v-icon>
                    </v-btn>
                    {{ title.name }}
                    </v-card-title>
            </template>
            <template v-else>
                <v-card-title class="subheading">
                    {{ title.name }}
                </v-card-title>
            </template>
            <v-divider></v-divider>
            <v-card-text style="height: 300px;">
              <transition name="fade">
                <v-list v-if="!loading">
                    <div
                        v-for="item in menu"
                        :key="item.id">
                        <template v-if="title.parent_id === item.parent_id && item.hasSubitems">
                            <v-list-tile
                            class="list-item"
                            @click="showItemChilds(item.id)">
                            <v-list-tile-content>
                                <v-list-tile-title>
                                {{ item.name }}
                                </v-list-tile-title>
                            </v-list-tile-content>
                            <v-list-tile-avatar class="text-lg-right">
                                <v-icon color="black">
                                    chevron_right
                                </v-icon>
                                </v-list-tile-avatar>
                            </v-list-tile>
                        </template>
                        <template v-else-if="title.parent_id === item.parent_id">
                            <v-list-tile
                            class="list-item">
                            <v-list-tile-content>
                                <v-list-tile-title>
                                {{ item.name }}
                                </v-list-tile-title>
                            </v-list-tile-content>
                            </v-list-tile>
                        </template>
                    </div>
                </v-list>
              </transition>
            </v-card-text>
            <v-divider></v-divider>
            <v-card-actions>
                <v-btn
                    color="blue darken-1"
                    flat
                    @click.native="rebuildMenu()">
                        Close
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script>
import { forEach, keysIn } from "lodash";
export default {
  name: "VDynamicMenu",
  props: {
    dialog: {
      type: Boolean,
      required: true
    },
    items: {
      type: Array,
      required: true,
      validator(array) {
        if (!(array.length > 0)) {
          console.log("Property 'items' is empty");
        }
        return array.length > 0;
      }
    },
    header: {
      type: Object,
      require: true,
      validator: function(object) {
        let counter = 0;
        forEach(keysIn(object), k => {
          if (k == "name") {
            counter++;
          }
          if (k == "parent_id") {
            counter++;
          }
        });
        if (counter < 2) {
          console.log(
            "Check 'header' component property contains 'name' and 'parent_id' properties"
          );
        }
        return counter === 2;
      }
    }
  },
  data() {
    return {
      title: {
        name: null,
        parent_id: null
      },
      menu: [],
      loading: false
    };
  },
  methods: {
    isItemFound(itemId, targetId, item) {
      if (itemId === targetId) {
        this.title.name = this.getTitle(item.parent_id);
        this.title.parent_id = item.parent_id;

        return true;
      }

      return false;
    },
    showItemChilds(parentId) {
      this.loading = true;
      setTimeout(() => {
        forEach(this.menu, item => {
          if (this.isItemFound(parentId, item.parent_id, item)) {
            return false;
          }
        });
        this.loading = false;
      }, 350);
    },
    returnToParentItem(parentId) {
      this.loading = true;
      setTimeout(() => {
        forEach(this.menu, item => {
          if (this.isItemFound(parentId, item.id, item)) {
            return false;
          }
        });
        this.loading = false;
      }, 350);
    },
    getTitle(id) {
      let name = null;
      forEach(this.menu, item => {
        if (id == item.id) {
          name = item.name;
          return false;
        }
      });

      if (name === null) {
        return "Categories";
      }
      return name;
    },
    rebuildMenu() {
      this.$emit("closeMe");
      this.menu = [];
      this.title = {
        name: "Categories",
        parent_id: null
      };
      this.buildMenu(this.items);
    },
    buildMenu(items) {
      forEach(items, item => {
        this.populateMenu(item, item.items.length > 0);
        if (item.items) {
          this.buildMenu(item.items);
        }
      });
    },
    populateMenu(item, hasSubitems) {
      const { id, name, parent_id } = item;
      this.menu.push({
        id,
        name,
        parent_id,
        hasSubitems
      });
    }
  },
  mounted() {
    this.buildMenu(this.items);
    this.title = this.header;
  }
};
</script>
