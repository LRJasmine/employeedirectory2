<script>
import axios from "axios";

const employeesURL = "http://localhost:3001/employees";

export default {
  data(){
    return{
      newemployee: {
          empNum: 0,
          firstName: '',
          lastName: '',
          gender: 'M',
          birthdate: ''
        },
      employeesdb: []
    }
  },
  async created() {
    try {
      const res = await axios.get(employeesURL);
      this.employeesdb = res.data;
    } catch (e) {
      console.error(e);
    }
  },
  methods: {
    async addEmployee() {
      try {

        const res = await axios.post(employeesURL, this.newemployee);

        this.employeesdb = [...this.employeesdb, res.data];
        this.newemployee = {};
        this.$swal({
          titleText:'New Employee successfully added',
          timer: 2000,
          icon: 'success',
          showConfirmButton: false,
          toast: true,
          width: '100em',
          position: 'top',
          customClass: {
            title: '!text-sm'
          }
        });
        this.$router.push("/");

      } catch (e) {
        console.error(e);
      }
    }
  }
}
</script>
<template>
  <div class="flex flex-col">
    <RouterLink 
      to="/" 
      class="self-center bg-[#90cdfc] border-0 rounded p-2 mt-5 font-bold mb-3 hover:bg-[#41749c] hover:text-white hover:border-[#6fa7d1] hover:border-0 hover:border-solid"
    >Return to Directory</RouterLink>
    <div class="newEmpContainer m-[2%] p-[2%] self-center flex items-center justify-center rounded-2xl bg-[#adbdc9] max-w-2xl">
      <form id="addNewEmpForm" @submit.prevent="addEmployee">
        <div class="formFields">
          <div>
            <label for="firstname">First Name</label>
            <input type="text" name="firstname" id="firstname" v-model="newemployee.firstName" required>
          </div>
          <div>
            <label for="lastname">Last Name</label>
            <input type="text" name="lastname" id="lastname" v-model="newemployee.lastName" required>
          </div>
        </div>
        <div class="formFields">
          <div>
            <label for="employeenum">Employee Number</label>
            <input type="number" name="employeenum" id="employeenum" v-model="newemployee.empNum" required>
          </div>
          <div>
            <label for="gender">Gender</label>
            <select name="gender" id="gender" v-model="newemployee.gender" required>
              <option value="M">M</option>
              <option value="F">F</option>
            </select>
          </div>
          <div>
            <label for="birthdate">Date of Birth</label>
            <input type="date" name="birthdate" id="birthdate" v-model="newemployee.birthdate" required>
          </div>
        </div>
        <div>
          <button 
            id="submitNewEmp" 
            class="submitEmp" 
            type="submit"
          >Add New Employee</button>
        </div>
      </form>
    </div>
  </div>
</template>