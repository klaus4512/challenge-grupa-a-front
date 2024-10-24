<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <h1>Consulta de alunos</h1>
      </v-col>
      <v-col cols="12" class="d-flex justify-space-between mb-4">
        <v-text-field
          v-model="search"
          label="Consulta de Alunos"
          class="mr-4"
          dense
          height="36px"
        ></v-text-field>
        <v-btn
          class="mr-4"
          color="primary"
          @click="loadStudents"
          dense
          height="36px"
        >
          Buscar
        </v-btn>
        <router-link to="/student/create">
          <v-btn color="primary">Cadastrar Aluno</v-btn>
        </router-link>
      </v-col>
      <v-col cols="12">
        <v-data-table
          :headers="headers"
          :items="students"
          class="elevation-1"
        >
          <template v-slot:item.actions="{ item }">
            <v-btn icon @click="editStudent(item)" variant="plain">
              <v-icon>mdi-pencil</v-icon>
            </v-btn>
            <delete :student="item"/>
          </template>
        </v-data-table>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
  import {ref, onBeforeMount, onMounted} from 'vue'
  import axios from "axios";
  import '@/components/student/delete.vue'
  import Delete from "@/components/student/delete.vue";

  const search = ref('')
  const students = ref([])

  const headers = [
    { title: 'Registro acadêmico', value: 'ra' },
    { title: 'Nome', value: 'name' },
    { title: 'CPF', value: 'cpf' },
    { title: 'Ações', value: 'actions', sortable: false },
  ]

  function loadStudents() {
    let params = {}
    if(search.value !== '') {
      params = {
        search: search.value
      }
    }
    axios.get('http://localhost:8000/api/students', {
      params: params
    }).then(response => {
        console.log(response.data.students)
        students.value = response.data.students
      })
      .catch(error => {
        console.error(error)
      })
  }
  const deleteStudent = (student) => {
    // Logic to delete the student
    console.log('Delete student', student)
  }

  onMounted(() => {
    loadStudents()
  })

</script>

<style scoped>
  .mb-4 {
    margin-bottom: 1rem;
  }
  .mr-4 {
    margin-right: 1rem;
  }
</style>
