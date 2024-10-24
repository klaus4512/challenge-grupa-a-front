<template>
  <v-btn icon @click="deleteStudent(student)" variant="plain">
    <v-icon>mdi-delete</v-icon>
  </v-btn>
</template>

<script setup lang="ts">
  import Swal from 'sweetalert2'
  import axios from "axios";

  const API_BASE_URL = import.meta.env.VITE_API_BASE_URL;

  const props = defineProps<{
    student: Object
  }>()

  const emit = defineEmits(['studentDeleted'])

  const deleteStudent = (student) => {
    Swal.fire({
      title: 'Deseja realmente excluir o aluno?',
      showCancelButton: true,
      icon: "question",
      confirmButtonText: 'Confirmar',
      cancelButtonText: 'Cancelar',
      customClass: {
        confirmButton: 'text-white',
        cancelButton: 'text-white'
      },
      confirmButtonColor: '#3085d6',
      cancelButtonColor: '#d33',
    }).then((result) => {
      if (result.isConfirmed) {
        axios.delete(`${API_BASE_URL}/students/${student.ra}`)
          .then(response => {
            if (response.status === 200) {
              emit('studentDeleted')
              Swal.fire({
                title: 'Sucesso!',
                text: 'Aluno excluído com sucesso!',
                icon: 'success',
                customClass: {
                  confirmButton: 'text-white',
                },
                confirmButtonText: 'OK'
              })
            }
          })
          .catch(error => {
            Swal.fire({
              title: 'Erro!',
              text: 'Ocorreu um erro ao excluir o aluno!',
              customClass: {
                confirmButton: 'text-white',
              },
              icon: 'error',
              confirmButtonText: 'OK'
            })
          })
      }
    })
  }
</script>

<style scoped>
  .text-white{
    color: white !important;
  }
</style>
