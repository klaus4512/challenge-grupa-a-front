<template>
  <v-layout class="rounded rounded-md">
    <v-app-bar>
      <v-sheet class="d-flex">
        <v-img
          :width="150"
          aspect-ratio="16/9"
          src="@/assets/1grupoa.png"
        ></v-img>
      </v-sheet>
    </v-app-bar>
    <v-navigation-drawer absolute permanent>
      <v-list-item
        title="Módulo Acadêmico"
      >
      </v-list-item>
      <v-divider></v-divider>
      <v-list
        :lines="false"
        density="compact"
        nav
      >
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :value="item"
          color="primary"
          :to="item.link"
        >
          <template v-slot:prepend>
            <v-icon :icon="item.icon"></v-icon>
          </template>
          <v-list-item-title v-text="item.text"></v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-main>
      <router-view />
    </v-main>
  </v-layout>
</template>

<script lang="ts" setup>
//
const items = [
  { icon: 'mdi-account', text: 'Alunos', link: '/student' },
]
</script>


