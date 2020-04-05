<template>
  <div id="student-table">
    <p v-if="students.length < 1" class="empty-table">No students</p>
    <table v-else>
      <thead>
        <tr>
          <th>First name</th>
          <th>Last name</th>
          <th>Gender</th>
          <th>Birth Date</th>
          <th>Hometown</th>
          <th>Department</th>
          <th>Student Id</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="student in students" v-bind:key="student.id">
          <td v-if="editing === student.id">
            <input type="text" v-model="student.firstName" />
          </td>
          <td v-else>{{student.firstName}}</td>
          <td v-if="editing === student.id">
            <input type="text" v-model="student.lastName" />
          </td>
          <td v-else>{{student.lastName}}</td>
          <td v-if="editing === student.id">
            <select v-model="student.gender">
              <option v-for="g in gender" v-bind:key="g">{{g.name}}</option>
            </select>
          </td>
          <td v-else>{{student.gender}}</td>
          <td v-if="editing === student.id">
            <input type="date" v-model="student.birthDate"/>
          </td>
          <td v-else>{{student.birthDate}}</td>
          <td v-if="editing === student.id">
            <input type="text" v-model="student.hometown"/>
          </td>
          <td v-else>{{student.hometown}}</td>
          <td v-if="editing === student.id">
            <input type="text" v-model="student.department"/>
          </td>
          <td v-else>{{student.department}}</td>
          <td v-if="editing === student.id">
            <input type="text" v-model="student.studentId"/>
          </td>
          <td v-else>{{student.studentId}}</td>
          <td v-if="editing === student.id">
            <button v-on:click="editStudent(student)">Save</button>
            <button class="muted-button" v-on:click="cancelEdit(student)">Cancel</button>
          </td>
          <td v-else>
            <button v-on:click="editMode(student)">Edit</button>
            <button v-on:click="$emit('delete:student', student.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "student-table",
  props: {
    students: Array
  },
  data() {
    return {
      editing: null,
      gender: [
        { id:1, name:"male" },
        { id:2, name:"female" }
      ]
    }
  },
  methods: {
    editMode(student) {
      this.cachedStudent = Object.assign({}, student)
      this.editing = student.id
    },
    cancelEdit(student) {
      Object.assign(student, this.cachedStudent)
      this.editing = null;
    },
    editStudent(student) {
      if (student.firstName === '' || student.lastName === '' || student.gender === '' || student.birthDate === '' || student.hometown === '' || student.department === '' || student.studentId === '') return
      this.$emit('edit:student', student.id, student)
      this.editing = null
    }
  }
};
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}
</style>