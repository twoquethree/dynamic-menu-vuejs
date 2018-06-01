<template>
  <v-app>
    <v-content >
      <v-container
        grid-list-xl 
        text-xs-center 
        fill-height>
        <v-container>
          <v-layout 
              align-center
              justify-center
              row
              wrap>
              <v-flex 
                xs12 
                sm3 
                md3 
                lg3>
                <v-dynamic-menu
                  :header="header" 
                  :items="items"
                  :dialog="dialog"
                  :returnedItem="item"
                  @closeMe="close"
                  @chosenItem="addItem"
                />
                <v-btn 
                  dark
                  slot="activator" 
                  color="primary"
                  @click="dialog = !dialog">
                  Open Menu
                </v-btn>
              </v-flex>
              <v-flex 
                xs12 
                sm3 
                md3 
                lg3>
                <v-card>
                  <v-subheader>Selected Items: </v-subheader>
                  <div class="text-xs-left">
                    <template v-for="item in selectedItems">
                      <v-chip 
                        :key="item.id"
                        @input="removeItem(item.id)"
                        close>
                      {{item.id}} - {{item.name}}
                    </v-chip>
                    </template>
                  </div>
                </v-card>
              </v-flex>
          </v-layout>
        </v-container>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import VDynamicMenu from "./VDynamicMenu";
import { remove } from "lodash";

export default {
  components: {
    VDynamicMenu
  },
  data() {
    return {
      selectedItems: [],
      dialog: false,
      item: null,
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
                  id: "2.1.2",
                  name: "Sales Skills",
                  items: [],
                  parent_id: "2.1"
                },
                {
                  id: "2.1.3",
                  name: "Lead Generation",
                  items: [],
                  parent_id: "2.1"
                },
                {
                  id: "2.1.4",
                  name: "Sales Funnel",
                  items: [],
                  parent_id: "2.1"
                },
                {
                  id: "2.1.5",
                  name: "CRM",
                  items: [],
                  parent_id: "2.1"
                }
              ],
              parent_id: "2"
            }
          ],
          parent_id: null
        }
      ],
      header: {
        name: "Categories",
        parent_id: null
      }
    };
  },
  methods: {
    addItem(value) {
      const { id, name, parent_id, hasSubitems, index } = value;
      const item = { id, name, parent_id, index, hasSubitems };
      this.selectedItems.push(item);
    },
    close() {
      this.dialog = !this.dialog;
    },
    removeItem(id) {
      const result = remove(this.selectedItems, i => i.id === id);
      const item = result[0];
      this.item = item;
    }
  }
};
</script>
