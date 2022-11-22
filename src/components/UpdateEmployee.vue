<script>
import axios from "axios";
const employeesURL = "http://localhost:3001/employees";

  export default {
    props:{
      employeeid: Number,
      employee: Object
    },
    data(){
      return {
        editemployee: this.employeeid,
        employeedata: this.employee
      }
    },
    methods: {
      async updateEmployee(){
        this.employeedata = this.employee
        const res = await axios.put(`${employeesURL}/${this.employeeid}`, this.employeedata);
        // this.employeedata = res.data;
        // console.log("empdata",this.employeedata);
        this.$emit('editComplete')
      },
      closeEditEmpForm(){
        this.$emit('hideForm', true)
      }
    }
  }
</script>
<template>
  <div class="newEmpContainer m-[2%] p-[2%] self-center flex items-center justify-center rounded-2xl bg-[#adbdc9] max-w-lg" id="editEmpFormContainer">
    <form @submit.prevent="updateEmployee" id="editEmpForm">
      <div class="formFields">
        <div>
          <label for="editfirstname">First Name</label>
          <input type="text" name="editfirstname" id="editfirstname" v-model="employee.firstName">
        </div>
        <div>
          <label for="editlastname">Last Name</label>
          <input type="text" name="editlastname" id="editlastname" v-model="employee.lastName">
        </div>
      </div>
      <div class="formFields">
        <div>
          <label for="editemployeenum">Employee Number</label>
          <input type="number" name="editemployeenum" id="editemployeenum" class="bg-gray-300 disabled" readonly v-model="employee.empNum">
        </div>
        <div>
          <label for="editgender">Gender</label>
          <select name="editgender" id="editgender" v-model="employee.gender">
            <option value="M">M</option>
            <option value="F">F</option>
          </select>
        </div>
        <div>
          <label for="editbirthdate">Date of Birth</label>
          <input type="date" name="editbirthdate" id="editbirthdate" v-model="employee.birthdate">
        </div>
      </div>
      <div class="formfooter">
        <button type="submit" id="editEmp" class="submitEmp">Update Employee</button>
        <button 
          type="reset" id="canceleditEmp" 
          @click="$emit('showForm', false)" 
          class="cancelUpdate py-2 px-5 bg-[#bd4444] text-[20px] rounded-lg border-0 hover:bg-[#9c0e0e] hover:text-white">Cancel</button>
      </div>
    </form>
  </div>
</template>