<template>
  <v-app>
    <v-content>
      <v-container
        grid-list-xl 
        fill-height>
        <v-layout 
          align-center
          justify-center>
          <v-flex xs3>
            <v-dynamic-menu
              :header="header" 
              :items="items"
              :dialog="dialog"
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
          <v-flex xs3>
            <v-card>
              <v-subheader>Selected Items: </v-subheader>
              <div class="text-xs-left">
                <v-chip 
                  v-for="item in selectedItems" 
                  :key="item.id" 
                  close>
                  {{item.id}} - {{item.name}}
                </v-chip>
              </div>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import VDynamicMenu from "./VDynamicMenu";
export default {
  components: {
    VDynamicMenu
  },
  data() {
    return {
      selectedItems: [],
      dialog: false,
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
      ],
      header: {
        name: "Categories",
        parent_id: null
      }
    };
  },
  methods: {
    addItem(value) {
      const { id, name } = value;
      const item = { id, name };
      this.selectedItems.push(item);
    },
    close() {
      this.dialog = !this.dialog;
    }
  }
};
</script>
