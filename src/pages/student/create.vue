<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <h1>Cadastrar Aluno</h1>
      </v-col>
      <v-col cols="12">
        <student-form/>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
  import StudentForm from '@/components/student/form.vue'
</script>

