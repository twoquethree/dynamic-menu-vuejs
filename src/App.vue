<template>
  <v-app>
    <v-content>
      <v-container fill-height fluid>
          <v-layout 
            justify-center
            align-center
            hidden-xs-only
            column>
            <v-card color="darken-2" class="elevation-3">
              <v-card-title>
                <template v-if="title.parent_id" >
                  <v-layout>
                    <v-flex d-inline>
                      <v-btn 
                        tag="a"
                        flat 
                        @click="previousItems(title.parent_id)">
                          <v-icon>
                            chevron_left
                          </v-icon>
                      </v-btn>
                      <span class="text-lg-right">
                        {{ title.name }}
                      </span>
                    </v-flex>
                  </v-layout>
                </template>
                <template v-else>
                  <v-card-text>
                    <p class="text-lg-center">
                      Categories
                    </p> 
                  </v-card-text>
                </template>
              </v-card-title>
                <v-divider></v-divider>
              <v-list>
                <template>
                  <div v-for="item in categories" :key="item.id">
                    <v-list-tile 
                      class="list-item" 
                      v-if="item.items.length > 0" 
                      @click="nextItems(item.id)">
                        <v-list-tile-title>
                          {{ item.name }}
                        </v-list-tile-title>
                        <v-list-tile-avatar class="text-lg-right">
                            <v-icon color="black">chevron_right</v-icon>
                        </v-list-tile-avatar>
                    </v-list-tile>
                    <v-list-tile class="list-item" v-else>
                      <v-list-tile-title>
                        {{ item.name }}
                      </v-list-tile-title>
                    </v-list-tile>
                  </div>
                 </template>
              </v-list>
            </v-card>
          </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import { find, head, filter } from "lodash";

export default {
  data() {
    return {
      title: {
        name: "Categories",
        current_level: 0
      },
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
                  parent_id: "1.1",
                  level: 2
                },
                {
                  id: "1.1.2",
                  name: "Vue",
                  items: [],
                  parent_id: "1.1",
                  level: 2
                },
                {
                  id: "1.1.3",
                  name: "Angular",
                  items: [],
                  parent_id: "1.1",
                  level: 2
                },
                {
                  id: "1.1.4",
                  name: "NodeJS",
                  items: [],
                  parent_id: "1.1",
                  level: 2
                },
                {
                  id: "1.1.5",
                  name: "React",
                  items: [],
                  parent_id: "1.1",
                  level: 2
                }
              ],
              parent_id: "1",
              level: 1
            },
            {
              id: "1.2",
              name: "Mobile development",
              items: [],
              parent_id: "1",
              level: 1
            },
            {
              id: "1.3",
              name: "Programming Languages",
              items: [
                {
                  id: "1.3.1",
                  name: "Python",
                  items: [],
                  parent_id: "1.3",
                  level: 2
                },
                {
                  id: "1.3.2",
                  name: "Java",
                  items: [],
                  parent_id: "1.3",
                  level: 2
                },
                {
                  id: "1.3.3",
                  name: "C#",
                  items: [],
                  parent_id: "1.3",
                  level: 2
                }
              ],
              parent_id: "1",
              level: 1
            }
          ],
          parent_id: null,
          level: 0
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
                  level: 2,
                  parent_id: "2.2"
                },
                {
                  id: "2.2.2",
                  name: "Lead Generation",
                  items: [],
                  level: 2,
                  parent_id: "2.2"
                },
                {
                  id: "2.2.3",
                  name: "Sales Funnel",
                  items: [],
                  level: 2,
                  parent_id: "2.2"
                },
                {
                  id: "2.2.4",
                  name: "CRM",
                  items: [],
                  level: 2,
                  parent_id: "2.2"
                }
              ],
              parent_id: "2"
            }
          ],
          parent_id: null,
          level: 0
        }
      ],
      displayItems: []
    };
  },
  methods: {
    nextItems(id) {
      const result = find(this.displayItems, i => i.id == id);
      if (result) {
        this.title.name = result.name;
        this.title.parent_id = head(result.items).parent_id;
        this.displayItems = result.items;
      }
    },
    previousItems(id) {
      this.displayItems = this.getItems(id, this.items);
    },
    getItems(id, items) {
      let result = [];
      for (const i in items) {
        const item = items[i];
        if (id === item.id) {
          if (item.parent_id === null) {
            result = items;
            break;
          } else {
            result = item.items;
            break;
          }
        } else if (item.items.length > 0) {
          result = this.getItems(id, item.items);
        }
      }
      return result;
    },
    getTitle(id, items) {
      const item = find(items, i => {
        if (i.id === id) {
          return {
            name: i.name,
            parent_id: i.parent_id
          };
        } else if (i.items) {
          this.getTitle(id, i.items);
        }
      });

      return item;
    }
  },
  computed: {
    categories() {
      return this.displayItems;
    }
  },
  mounted() {
    this.displayItems = this.items;
  }
};
</script>
