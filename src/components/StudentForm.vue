<template>
  <div id="student-form">
    <form v-on:submit.prevent="handleSubmit">
      <label>First name</label>
      <input
        type="text"
        v-bind:class="{ 'has-error': submitting && invalidFirstName }"
        v-model="student.firstName"
        v-on:focus="clearStatus"
        v-on:keypress="clearStatus"
      />
      <label>Last name</label>
      <input
        type="text"
        v-bind:class="{ 'has-error': submitting && invalidLastName }"
        v-model="student.lastName"
        v-on:focus="clearStatus"
      />
      <label>Gender</label>
      <select
              v-bind:class="{ 'has-error': submitting && invalidGender }"
              v-model="student.gender"
              v-on:focus="clearStatus"
      >
        <option v-for="g in gender" v-bind:key="g.id">{{g.name}}</option>
      </select>
      <label>Birth date</label>
      <input
              type="date"
              v-bind:class="{ 'has-error': submitting && invalidBirthDate }"
              v-model="student.birthDate"
              v-on:focus="clearStatus"
      />
      <label>Hometown</label>
      <input
              type="text"
              v-bind:class="{ 'has-error': submitting && invalidHometown }"
              v-model="student.hometown"
              v-on:focus="clearStatus"
      />
      <label>Department</label>
      <input
              type="text"
              v-bind:class="{ 'has-error': submitting && invalidDepartment }"
              v-model="student.department"
              v-on:focus="clearStatus"
      />
      <label>Student Id</label>
      <input
              type="text"
              v-bind:class="{ 'has-error': submitting && invalidStudentId }"
              v-model="student.studentId"
              v-on:focus="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
      <p v-if="success" class="success-message">✅ Student successfully added</p>
      <button>Add Student</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "student-form",
  computed: {
    invalidFirstName() {
      return this.student.firstName === "";
    },
    invalidLastName() {
      return this.student.lastName === "";
    },
    invalidGender() {
      return this.student.gender === "";
    },
    invalidBirthDate() {
      return this.student.birthDate === "";
    },
    invalidHometown() {
      return this.student.hometown === "";
    },
    invalidDepartment() {
      return this.student.department === "";
    },
    invalidStudentId() {
      return this.student.studentId === "";
    }
  },
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      student: {
        firstName: "",
        lastName: "",
        gender: "",
        birthDate: "",
        hometown: "",
        department: "",
        studentId: "",
      },
      gender: [
        { id:1, name:"male" },
        { id:2, name:"female" }
      ]
    };
  },
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (this.invalidFirstName || this.invalidLastName || this.invalidGender || this.invalidBirthDate || this.invalidHometown || this.department || this.studentId) {
        this.error = true;
        return;
      }

      this.$emit("add:student", this.student);
      this.student = {
        firstName: "",
        lastName: "",
        gender: "",
        birthDate: "",
        hometown: "",
        department: "",
        studentId: ""
      };
      this.error = false;
      this.success = true;
      this.submitting = false;
    },

    clearStatus() {
      this.success = false;
      this.error = false;
    }
  }
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}

[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>