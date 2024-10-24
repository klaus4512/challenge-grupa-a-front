<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <h1>Consulta de alunos</h1>
      </v-col>
      <v-col cols="12" class="d-flex justify-space-between align-center">
        <v-form @submit.prevent="loadStudents" class="d-flex align-center">
          <v-text-field
            v-model="search"
            label="Consulta de Alunos"
            class="mr-2"
            dense
            height="36px"
            width="30vw"
          ></v-text-field>
          <v-btn
            color="primary"
            type="submit"
            dense
            height="36px"
          >
            Buscar
          </v-btn>
        </v-form>
        <router-link to="/student/create">
          <v-btn color="primary">Cadastrar Aluno</v-btn>
        </router-link>
      </v-col>
      <v-col cols="12">
        <v-data-table
          :headers="headers"
          :items="students"
          class="elevation-1"
          :footer-props="{
            itemsPerPageText: 'Itens por página:',
            itemsPerPageAllText: 'Todos',
          }"
        >
          <template v-slot:item.actions="{ item }">
            <router-link :to="{ name: 'student-edit', params: { ra: item.ra } }">
              <v-btn icon variant="plain">
                <v-icon>mdi-pencil</v-icon>
              </v-btn>
            </router-link>
            <delete :student="item" @studentDeleted="loadStudents"/>
          </template>
        </v-data-table>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
  import {ref, onMounted} from 'vue'
  import axios from "axios";
  import '@/components/student/delete.vue'
  import Delete from "@/components/student/delete.vue";
  import {Mask} from "maska";
  import {Student} from "@/types/student";

  let mask = new Mask({mask: '###.###.###-##'})

  const API_BASE_URL = import.meta.env.VITE_API_BASE_URL;

  const search = ref('')
  const students = ref<Student[]>([]);

  const headers = [
    { title: 'Registro acadêmico', value: 'ra' },
    { title: 'Nome', value: 'name' },
    { title: 'CPF', value: 'cpf' },
    { title: 'Ações', value: 'actions', sortable: false },
  ]

  const loadStudents = () => {
    let params = {}
    if(search.value !== '') {
      params = {
        search: search.value
      }
    }
    axios.get(`${API_BASE_URL}/students`, {
      params: params
    }).then(response => {
        students.value = response.data.students.map((student: Student) => {
          student.cpf = mask.masked(student.cpf)
          return student
        })
      }).catch(error => {
        console.error(error)
      })
  }

  onMounted(() => {
    loadStudents()
  })
</script>
