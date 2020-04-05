<template>
  <div id="app" class="medium-container">
    <h1>Students</h1>
    <student-form v-on:add:student="addStudent" />
    <student-table
      v-bind:students="students"
      v-on:delete:student="deleteStudent"
      v-on:edit:student="editStudent"
    />
  </div>
</template>

<script>
import StudentTable from "./components/StudentTable.vue";
import StudentForm from "./components/StudentForm.vue";

export default {
  name: "App",
  components: {
    StudentTable,
    StudentForm
  },
  data() {
    return {
      students: [
        {
          id: 1,
          firstName: "Richard",
          lastName: "Hendricks",
          gender: "male",
          birthDate: "1992-1-1",
          hometown: "Tokyo",
          department: "Computer Science & Technology",
          studentId: "163819342"
        },
        {
          id: 2,
          firstName: "Bertram",
          lastName: "Gilfoyle",
          gender: "female",
          birthDate: "1923-3-12",
          hometown: "Paris",
          department: "Physics",
          studentId: "183042872"
        },
        {
          id: 3,
          firstName: "Dinesh",
          lastName: "Chugtai",
          gender: "male",
          birthDate: "2023-9-10",
          hometown: "Beijing",
          department: "Psychology",
          studentId: "193204202"
        }
      ]
    };
  },

  mounted() {
    this.getStudents();
  },

  methods: {
    editStudent(id, updatedStudent) {
      this.students = this.students.map(student =>
        student.id === id ? updatedStudent : student
      );
    },
    deleteStudent(id) {
      this.students = this.students.filter(student => student.id !== id);
    },
    addStudent(student) {
      const lastId =
        this.students.length > 0
          ? this.students[this.students.length - 1].id
          : 0;
      const id = lastId + 1;
      const newStudent = { ...student, id };

      this.students = [...this.students, newStudent];
    },

    async getStudents() {
      try {
        const response = await fetch("http://localhost:8083/students");
        const data = await response.json();
        this.students = data._embedded.students;
      } catch (error) {
        console.error(error);
      }
    }
  }
};
</script>

<style>
button {
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}

.medium-container {
  max-width: 1200px;
}
</style>
