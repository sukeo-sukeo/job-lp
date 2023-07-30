<script setup>

</script>

<template>
<v-app-bar :elevation="1">
  <v-col cols="4">
    <v-text-field append-inner-icon="mdi-magnify" variant="underlined"></v-text-field>
  </v-col>
  <v-spacer></v-spacer>
  <v-col cols="auto">
    <h1 class="text-uppercase">
      Fujikei Wine
    </h1>
  </v-col>
  <v-spacer></v-spacer>
  <v-col cols="auto">
    <v-list class="d-flex">
      <v-list-item>
        menu1
      </v-list-item>
      <v-list-item>
        menu2
      </v-list-item>
      <v-list-item>
        menu3
      </v-list-item>
    </v-list>
  </v-col>
</v-app-bar>
</template>