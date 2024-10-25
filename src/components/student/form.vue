<template>
  <v-form ref="form" v-model="valid" @submit.prevent="submitForm">
    <v-text-field
      v-model="student.name"
      label="Nome"
      :rules="[rules.required]"
      :error-messages="serverValidationErrors.name"
      required
    ></v-text-field>
    <v-text-field
      v-model="student.email"
      label="Email"
      :rules="[rules.required, rules.email]"
      :error-messages="serverValidationErrors.email"
      required
    ></v-text-field>
    <v-text-field
      v-model="student.ra"
      label="Registro Acadêmico"
      :rules="[rules.required, rules.numeric]"
      v-maska="'###############'"
      :error-messages="serverValidationErrors.ra"
      :disabled="editMode"
      required
    ></v-text-field>
    <v-text-field
      v-model="student.cpf"
      label="CPF"
      :rules="[rules.required, rules.cpf]"
      v-maska="'###.###.###-##'"
      :error-messages="serverValidationErrors.cpf"
      :disabled="editMode"
      required
    ></v-text-field>
    <v-row justify="end" class="mt-3">
      <v-col cols="auto">
        <router-link to="/student">
          <v-btn color="red" class="mr-2">Cancelar</v-btn>
        </router-link>
        <v-btn color="primary" type="submit">Salvar</v-btn>
      </v-col>
    </v-row>
  </v-form>
</template>

<script setup lang="ts">
  import {ref, watch} from 'vue'
  import axios from 'axios'
  import Swal from 'sweetalert2'
  import { vMaska } from "maska/vue"
  import { FormProps } from "@/types/FormProps";

  const API_BASE_URL = import.meta.env.VITE_API_BASE_URL;

  const props = defineProps<FormProps>()

  const valid = ref(false)
  const form = ref(null)

  const student = ref({
    name: props.student?.name || '',
    ra: props.student?.ra || '',
    cpf: props.student?.cpf?.number || '',
    email: props.student?.email?.address || ''
  })

  watch(() => props.student, () => {
    student.value = {
      name: props.student?.name || '',
      ra: props.student?.ra || '',
      cpf: props.student?.cpf?.number || '',
      email: props.student?.email?.address || ''
    }
  })

  const serverValidationErrors = ref({
    name: '',
    ra: '',
    cpf: '',
    email: ''
  })

  const rules = {
    required: (value: string) => !!value || 'Campo obrigatório',
    numeric: (value: string) => /^\d+$/.test(value) || 'Apenas números são permitidos',
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
    form.value.reset()
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
      if (props.editMode) {
        update()
      } else {
        create()
      }
    }
  }

  const create = () => {
    axios.post(`${API_BASE_URL}/students`, student.value)
      .then(response => {
        if (response.status === 201) {
          Swal.fire({
            title: 'Sucesso!',
            text: 'Aluno cadastrado com sucesso!',
            icon: 'success',
            customClass: {
              confirmButton: 'text-white',
            },
            confirmButtonText: 'OK'
          })
        }
        resetForm()
        resetErrors()
      })
      .catch(error => {
        if (error.response.status === 422) {
          console.log(error.response.data.errors.ra)
          serverValidationErrors.value.cpf = error.response.data.errors?.cpf
          serverValidationErrors.value.name = error.response.data.errors?.name
          serverValidationErrors.value.ra = error.response.data.errors?.ra
          serverValidationErrors.value.email = error.response.data.errors?.email
        }else{
          Swal.fire({
            title: 'Erro!',
            text: 'Ocorreu um erro ao cadastrar o aluno!',
            icon: 'error',
            customClass: {
              confirmButton: 'text-white',
            },
            confirmButtonText: 'OK'
          })
          console.log(error)
        }
      })
  }

  const update = () => {
    axios.put(`${API_BASE_URL}/students/${student.value.ra}`, student.value)
      .then(response => {
        if (response.status === 200) {
          Swal.fire({
            title: 'Sucesso!',
            text: 'Aluno atualizado com sucesso!',
            icon: 'success',
            customClass: {
              confirmButton: 'text-white',
            },
            confirmButtonText: 'OK'
          })
        }
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
            text: 'Ocorreu um erro ao atualizar o aluno!',
            icon: 'error',
            confirmButtonText: 'OK',
            customClass: {
              confirmButton: 'text-white',
            },
          })
          console.log(error)
        }
      })
  }
</script>


<style scoped>
  .text-white{
    color: white !important;
  }
</style>
