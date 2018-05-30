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
              <div v-for="(value, key) in levels" :key="key">
                <div v-for="item in value" :key="item.id">
                   <template v-if="title.level === item.level && title.parent_id === item.parent_id">
                      <v-list-tile
                        class="list-item"
                        >
                        <v-list-tile-title>
                          {{ item.name }}
                        </v-list-tile-title>
                        <v-list-tile-avatar class="text-lg-right">
                          <v-icon color="black">
                            chevron_right
                            </v-icon>
                        </v-list-tile-avatar>
                      </v-list-tile>
                   </template>
                   <template v-else-if="title.level === item.level && title.parent_id === item.parent_id">
                     <v-list-tile 
                      class="list-item"
                      >
                      <v-list-tile-title>
                        {{ item.name }}
                      </v-list-tile-title>
                    </v-list-tile>
                   </template>
                </div>
              </div>
               </v-list>
            </v-card>
          </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import { find, head, filter, isEmpty } from "lodash";

export default {
  data() {
    return {
      title: {
        name: "Categories",
        level: 1,
        parent_id: null
      },
      levels: {},
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
                  level: 3
                },
                {
                  id: "1.1.2",
                  name: "Vue",
                  items: [],
                  parent_id: "1.1",
                  level: 3
                },
                {
                  id: "1.1.3",
                  name: "Angular",
                  items: [],
                  parent_id: "1.1",
                  level: 3
                },
                {
                  id: "1.1.4",
                  name: "NodeJS",
                  items: [],
                  parent_id: "1.1",
                  level: 3
                },
                {
                  id: "1.1.5",
                  name: "React",
                  items: [],
                  parent_id: "1.1",
                  level: 3
                }
              ],
              parent_id: "1",
              level: 2
            },
            {
              id: "1.2",
              name: "Mobile development",
              items: [],
              parent_id: "1",
              level: 2
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
                  level: 3
                },
                {
                  id: "1.3.2",
                  name: "Java",
                  items: [],
                  parent_id: "1.3",
                  level: 3
                },
                {
                  id: "1.3.3",
                  name: "C#",
                  items: [],
                  parent_id: "1.3",
                  level: 3
                }
              ],
              parent_id: "1",
              level: 2
            }
          ],
          parent_id: null,
          level: 1
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
                  parent_id: "2.1",
                  level: 3
                },
                {
                  id: "2.2.1",
                  name: "Sales Skills",
                  items: [],
                  parent_id: "2.2",
                  level: 3
                },
                {
                  id: "2.2.2",
                  name: "Lead Generation",
                  items: [],
                  parent_id: "2.2",
                  level: 3
                },
                {
                  id: "2.2.3",
                  name: "Sales Funnel",
                  items: [],
                  parent_id: "2.2",
                  level: 3
                },
                {
                  id: "2.2.4",
                  name: "CRM",
                  items: [],
                  parent_id: "2.2",
                  level: 3
                }
              ],
              parent_id: "2",
              level: 2
            }
          ],
          parent_id: null,
          level: 1
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
    },
    buildLevels(items) {
      for (const i in items) {
        const item = items[i];
        this.fillLevels(item, item.items.length > 0);
        if (item.items) {
          this.buildLevels(item.items);
        }
      }
    },
    fillLevels(item, hasSubitems) {
      const key = item.level;
      const { id, name, level, parent_id } = item;
      if (!(key in this.levels)) {
        this.levels[key] = [
          {
            id,
            name,
            level,
            parent_id,
            hasSubitems
          }
        ];
      } else {
        this.levels[key].push({
          id,
          name,
          level,
          parent_id,
          hasSubitems
        });
      }
    }
  },
  computed: {
    categories() {
      return this.levels;
    }
  },
  mounted() {
    this.displayItems = this.items;
    this.buildLevels(this.items);
  }
};
</script>
