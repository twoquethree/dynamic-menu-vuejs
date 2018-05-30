<template>
  <v-app>
    <v-content>
      <v-container 
        fill-height 
        fluid>
        <v-layout
          justify-center
          align-center
          hidden-xs-only>
          <v-dialog 
            v-model="dialog" 
            scrollable 
            max-width="300px">
            <v-btn slot="activator" color="primary" dark>Open Menu</v-btn>
             <v-card> 
              <template v-if="title.parent_id" >
                <v-layout>
                  <v-flex items-center>
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
                  </v-flex>
                </v-layout>
              </template>
              <template v-else>
                <v-card-title class="ma-2 subheading">
                  {{ title.name }}
                </v-card-title>
              </template>
              <v-divider></v-divider>
              <v-card-text style="height: 300px;">
                <v-list transition="fade-transition" >
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
              </v-card-text>
              <v-divider></v-divider>
              <v-card-actions>
                <v-btn color="blue darken-1" flat @click.native="rebuildMenu()">Close</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import { forEach } from "lodash";

export default {
  data() {
    return {
      dialog: false,
      title: {
        name: "Categories",
        parent_id: null
      },
      menu: [],
      items: [
        {
          id: "1",
          name: "Development",
          items: [
            {
              id: "1.1",
              name: "Web development",
              items: [
                {
                  id: "1.1.1",
                  name: "Javascript",
                  items: [],
                  parent_id: "1.1"
                },
                {
                  id: "1.1.2",
                  name: "Vue",
                  items: [
                    {
                      id: "1.1.2.1",
                      name: "Vuex",
                      items: [],
                      parent_id: "1.1.2"
                    },
                    {
                      id: "1.1.2.2",
                      name: "VueRouter",
                      items: [],
                      parent_id: "1.1.2"
                    },
                    {
                      id: "1.1.2.3",
                      name: "Vue NativeScript",
                      items: [],
                      parent_id: "1.1.2"
                    }
                  ],
                  parent_id: "1.1"
                },
                {
                  id: "1.1.3",
                  name: "Angular",
                  items: [],
                  parent_id: "1.1"
                },
                {
                  id: "1.1.4",
                  name: "NodeJS",
                  items: [],
                  parent_id: "1.1"
                },
                {
                  id: "1.1.5",
                  name: "React",
                  items: [],
                  parent_id: "1.1"
                }
              ],
              parent_id: "1"
            },
            {
              id: "1.2",
              name: "Mobile development",
              items: [],
              parent_id: "1"
            },
            {
              id: "1.3",
              name: "Programming Languages",
              items: [
                {
                  id: "1.3.1",
                  name: "Python",
                  items: [],
                  parent_id: "1.3"
                },
                {
                  id: "1.3.2",
                  name: "Java",
                  items: [],
                  parent_id: "1.3"
                },
                {
                  id: "1.3.3",
                  name: "C#",
                  items: [],
                  parent_id: "1.3"
                }
              ],
              parent_id: "1"
            }
          ],
          parent_id: null
        },
        {
          id: "2",
          name: "Business",
          items: [
            {
              id: "2.1",
              name: "Finance",
              items: [
                {
                  id: "2.1.1",
                  name: "Financial Analysis",
                  items: [],
                  parent_id: "2.1"
                },
                {
                  id: "2.2.1",
                  name: "Sales Skills",
                  items: [],
                  parent_id: "2.2"
                },
                {
                  id: "2.2.2",
                  name: "Lead Generation",
                  items: [],
                  parent_id: "2.2"
                },
                {
                  id: "2.2.3",
                  name: "Sales Funnel",
                  items: [],
                  parent_id: "2.2"
                },
                {
                  id: "2.2.4",
                  name: "CRM",
                  items: [],
                  parent_id: "2.2"
                }
              ],
              parent_id: "2"
            }
          ],
          parent_id: null
        }
      ]
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
    rebuildMenu() {
      this.dialog = false;
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
  }
};
</script>
