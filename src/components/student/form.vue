<template>
  <v-form ref="form" v-model="valid" @submit.prevent="submitForm">
    <v-text-field
      v-model="student.name"
      label="Nome"
      :rules="[rules.required]"
      required
    ></v-text-field>
    <v-text-field
      v-model="student.email"
      label="Email"
      :rules="[rules.required, rules.email]"
      required
    ></v-text-field>
    <v-text-field
      v-model="student.ra"
      label="Registro Acadêmico"
      :rules="[rules.required]"
      required
    ></v-text-field>
    <v-text-field
      v-model="student.cpf"
      label="CPF"
      :rules="[rules.required, rules.cpf]"
      v-maska="'###.###.###-##'"
      :error-messages="serverValidationErrors.cpf"
      required
    ></v-text-field>
    <router-link to="/student">
      <v-btn color="red" class="mr-4">Cancelar</v-btn>
    </router-link>
    <v-btn color="primary" type="submit">Salvar</v-btn>
  </v-form>
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import axios from 'axios'
  import Swal from 'sweetalert2'
  import { vMaska } from "maska/vue"

  const valid = ref(false)

  const student = ref({
    name: '',
    ra: '',
    cpf: '',
    email: ''
  })

  const serverValidationErrors = ref({
    name: '',
    ra: '',
    cpf: '',
    email: ''
  })

  const rules = {
    required: (value: string) => !!value || 'Campo obrigatório',
    cpf: (value: string) => /^\d{3}\.\d{3}\.\d{3}-\d{2}$/.test(value) || 'CPF inválido',
    email: (value: string) => /.+@.+\..+/.test(value) || 'Email inválido'
  }

  const resetForm = () => {
    student.value = {
      name: '',
      ra: '',
      cpf: '',
      email: ''
    }
  }

  const resetErrors = () => {
    serverValidationErrors.value = {
      name: '',
      ra: '',
      cpf: '',
      email: ''
    }
  }


  const submitForm = () => {
    if (valid.value) {
      axios.post('http://localhost:8000/api/students', student.value)
        .then(response => {
          if (response.status === 201) {
            Swal.fire({
              title: 'Sucesso!',
              text: 'Aluno cadastrado com sucesso!',
              icon: 'success',
              confirmButtonText: 'OK'
            })
          }
          resetForm()
          resetErrors()
        })
        .catch(error => {
          if (error.response.status === 422) {
            serverValidationErrors.value.cpf = error.response.data.errors?.cpf
            serverValidationErrors.value.name = error.response.data.errors?.name
            serverValidationErrors.value.ra = error.response.data.errors?.ra
            serverValidationErrors.value.email = error.response.data.errors?.email
          }else{
            Swal.fire({
              title: 'Erro!',
              text: 'Ocorreu um erro ao cadastrar o aluno!',
              icon: 'error',
              confirmButtonText: 'OK'
            })
            console.log(error)
          }
        })
    }
  }
</script>


<style scoped>
  .mb-4 {
    margin-bottom: 1rem;
  }
  .mr-4 {
    margin-right: 1rem;
  }
</style>
