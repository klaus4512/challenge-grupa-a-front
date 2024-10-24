<template>
  <v-btn icon @click="deleteStudent(student)" variant="plain">
    <v-icon>mdi-delete</v-icon>
  </v-btn>
</template>

<script setup lang="ts">
  import Swal from 'sweetalert2'

  const props = defineProps<{
    student: Object
  }>()

  const deleteStudent = (student) => {
    Swal.fire({
      title: 'Deseja realmente excluir o aluno?',
      showCancelButton: true,
      confirmButtonText: 'Confirmar',
      cancelButtonText: 'Cancelar',
      confirmButtonColor: '#3085d6',
      cancelButtonColor: '#d33',
    }).then((result) => {
      if (result.isConfirmed) {
        console.log('Delete student', student)
      }
    })
  }
</script>
