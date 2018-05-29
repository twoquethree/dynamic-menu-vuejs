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
              <v-card-title primary-title>
                <div class="subheading">Unlimited music now</div>
              </v-card-title>
               <v-list>
                  <v-list-tile v-for="n in 5" :key="n">
                    <v-list-tile-title v-text="'Item ' + n"></v-list-tile-title>
                  </v-list-tile>
                </v-list>
            </v-card>
          </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
export default {
  data() {
    return {};
  }
};
</script>
