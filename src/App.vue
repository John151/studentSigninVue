<template>
  <div id="app">

    <new-student-form v-on:student-added="newStudentAdded"></new-student-form>
    <student-table v-bind:students="students"
                   v-on:student-arrived-or-left="studentArrivedOrLeft"
                   v-on:delete-student="studentDeleted"></student-table>
    <student-message v-bind:student="mostRecentStudent"></student-message>

  </div>
</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue';
import StudentMessage from "@/components/StudentMessage";
import StudentTable from "@/components/StudentTable";

export default {
  name: 'App',
  components: {
    NewStudentForm,
    StudentMessage,
    StudentTable
  },
  data() {
    return {
      students: [],
      mostRecentStudent: {}
    }
  },
  methods: {
    newStudentAdded(student) {
      this.students.push(student)
      this.students.sort( function (s1, s2) {
        return s1.name.toLowerCase() < s2.name.toLowerCase() ? -1 : 1
      })
    },
    studentArrivedOrLeft(student, present) {
      let updateStudent = this.students.find( function(s) {
        if (s.name === student.name && s.starID === student.starID) {
          return true
        }
      })

      if (updateStudent) {
        updateStudent.present = present
        this.mostRecentStudent = student
      }
    },
    studentDeleted(student) {
        this.students = this.students.filter( function(s) {
            if (s != student) {
                return true
            }
        })
        this.mostRecentStudent = {}
      }
  }
}
</script>

<style>

</style>
