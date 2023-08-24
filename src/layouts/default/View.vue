<template>
  <v-main class="bg-grey-lighten-3">
      <v-container>
        <v-row>
          <v-col cols="2">
            <v-sheet rounded="lg">
              <v-list rounded="lg">
                <v-list-item
                  v-for="item in items"
                  :key="item.title"
                  link
                  :to="item.to"
                  color="primary"
                  variant="plain"
                >

                <template v-slot:prepend>
                  <v-icon :icon="item.icon"></v-icon>
                </template>

                <v-list-item-title>{{ item.title }}</v-list-item-title>
              </v-list-item>

              </v-list>
            </v-sheet>
          </v-col>

          <v-col>
            <v-sheet
              min-height="80vh"
              rounded="lg"
            >
            <v-breadcrumbs :items="items"></v-breadcrumbs>
            <router-view />
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
</template>

<script setup>
  //
</script>

<script>
  export default {
    data: () => ({
      items: [
        { title: 'Home', icon: 'mdi-home', to: '/'},
        { title: 'Service Report', icon: 'mdi-file-chart', to: '/ServiceReport'},
        { title:'HOP Report', icon: 'mdi-file-chart', to:'/HopReport'}
      ],
    }),
  }
</script>