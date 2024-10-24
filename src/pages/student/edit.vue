<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <h1>Edição de aluno</h1>
      </v-col>
      <v-col cols="12">
        <student-form :student="student" :edit-mode="true"/>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
  import StudentForm from '@/components/student/form.vue'
  import {onBeforeMount, ref} from 'vue'
  import { useRoute } from 'vue-router'
  import axios from 'axios'

  const API_BASE_URL = import.meta.env.VITE_API_BASE_URL;

  const route = useRoute()
  const studentRa = route.params.ra

  const student = ref({})
  const fetchStudent = () => {
      axios.get(`${API_BASE_URL}/students/${studentRa}`).then(response => {
        console.log(response.data)
        student.value = response.data.student
      }).catch(error => {
        console.error('Error fetching student:', error)
      })
  }

  onBeforeMount(() => {
    fetchStudent()
  })
</script>

