<template>
    <v-dialog
        v-model="dialog"
        persistent
        scrollable
        max-width="300px">
        <v-card>
            <template v-if="title.id" >
              <v-card-title class="subheading">
                <v-btn
                  icon
                  flat
                  @click="returnToParentItem(title.id)">
                  <v-icon>
                  chevron_left
                  </v-icon>
                </v-btn>
                  {{ title.id }} - {{ title.name }}
              </v-card-title>
            </template>
            <template v-else>
              <v-card-title class="subheading">
                <v-icon 
                  id="ico-header" 
                  color="black">
                  menu
                </v-icon>
                   {{ title.id }} {{ title.name }}
              </v-card-title>
            </template>
            <v-divider></v-divider>
            <v-card-text style="height: 300px;">
                <v-list>
                  <transition-group name="fade" mode="in-out">
                    <template v-for="(item, index) in displayItems">
                      <v-list-tile
                        :key="index"
                        v-if="item.hasSubitems"
                        class="list-item"
                        @click="showItemChilds(item.id)"
                        :disabled="item.itemsCount <= 0">
                        <v-list-tile-content>
                            <v-list-tile-title>
                           {{ item.id }} - {{ item.name }}
                            </v-list-tile-title>
                        </v-list-tile-content>
                        <v-list-tile-avatar class="text-lg-right">
                            <v-icon color="black">
                                chevron_right
                            </v-icon>
                            </v-list-tile-avatar>
                        </v-list-tile>
                        <v-list-tile
                          v-else-if="title.id === item.parent_id"
                          :key="index"
                          class="list-item"
                          @click="removeItem(item.id)">
                        <v-list-tile-content>
                            <v-list-tile-title>
                            {{ item.id }} - {{ item.name }}
                            </v-list-tile-title>
                        </v-list-tile-content>
                      </v-list-tile>
                    </template>
                    </transition-group>
                </v-list>
            </v-card-text>
            <v-divider></v-divider>
            <v-card-actions>
              <v-spacer></v-spacer>
                <v-btn
                    color="blue darken-1"
                    flat
                    @click.native="close()">
                        Close
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script>
import { forEach, keysIn, remove, clone, find, filter } from "lodash";

export default {
  name: "VDynamicMenu",
  props: {
    returnedItem: {
      type: Object
    },
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
        id: null
      },
      displayItems: [],
      menu: []
    };
  },
  methods: {
    isItemFound(itemId, targetId, item) {
      if (itemId === targetId) {
        this.title.name = this.getTitle(item.parent_id);
        this.title.id = item.parent_id;
        this.displayItems = [];
        setTimeout(() => {
          this.displayItems = filter(this.menu, i => {
            return this.title.id === i.parent_id;
          });
        }, 500);

        return true;
      }

      return false;
    },
    showItemChilds(parentId) {
      forEach(this.menu, item => {
        if (this.isItemFound(parentId, item.parent_id, item)) {
          return false;
        }
      });
    },
    returnToParentItem(parentId) {
      forEach(this.menu, item => {
        if (this.isItemFound(parentId, item.id, item)) {
          return false;
        }
      });
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
    close() {
      this.$emit("closeMe");
      this.title = clone(this.header);
      this.showItemChilds(null);
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
      this.menu.push({
        id: item.id,
        name: item.name,
        parent_id: item.parent_id,
        itemsCount: item.items.length,
        hasSubitems
      });
    },
    removeItem(id) {
      const result = remove(this.menu, (m, index) => {
        if (m.id === id) {
          m["index"] = index;
          return m;
        }
      });
      const item = result[0];
      forEach(this.menu, m => {
        if (m.id === item.parent_id) {
          m.itemsCount -= 1;
        }
      });
      this.$emit("chosenItem", item);
      this.close();
    }
  },
  watch: {
    returnedItem: function(val) {
      if (val !== null) {
        forEach(this.menu, m => {
          if (m.id === val.parent_id) {
            m.itemsCount++;
          }
        });

        this.menu.splice(val.index, 0, val);
        this.showItemChilds(null);
      }
    }
  },
  mounted() {
    this.buildMenu(this.items);
    this.title = clone(this.header);
    this.showItemChilds(null);
  }
};
</script>

<style scoped>
.fade-leave,
.fade-enter {
  transform: translateX(10px);
}
.fade-leave-to {
  opacity: 0;
}
.fade-enter-to {
  opacity: 1;
}

.fade-leave-active,
.fade-enter-active {
  transition: all 0.8s ease;
}

.card__title {
  height: 80px !important;
}

#ico-header {
  padding-left: 15px !important;
  padding-right: 15px !important;
}
</style>
