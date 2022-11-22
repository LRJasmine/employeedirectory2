<script>
import axios from "axios";

const employeesURL = "http://localhost:3001/employees";

import UpdateEmployee from './UpdateEmployee.vue'
export default {
  components: {
    UpdateEmployee
  },
  data() {
    return {
      employeesdb: [],
      employeeid: 0,
      employee: {},
      showUpdateForm: false,
    }
  },
  async created() {
    try {
      const res = await axios.get(`http://localhost:3001/employees`);
      this.employeesdb = res.data.sort((emp1, emp2) => (emp1.empNum > emp2.empNum) ? 1 : (emp1.empNum < emp2.empNum) ? -1 : 0);

    } catch (e) {
      console.error(e);
    }
  },
  methods: {
    toggleForm(show){
      this.showUpdateForm = show;
    },
    async editEmployee(empnum){
      this.employeeid = empnum;
      const res = await axios.get(`${employeesURL}/${empnum}`);
      this.employee = res.data;
      this.toggleForm(true);
    },
    async editComplete(){
      try {
        this.toggleForm(false);
        this.$swal({
          titleText:'Employee successfully updated',
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
        const res = await axios.get(`http://localhost:3001/employees`);
        this.employeesdb = res.data;
      } catch (e) {
        console.error(e);
      }
    },
    async deleteEmployee(empnum){
      try {
        this.$swal({
          titleText:'Are you sure you want to delete this employee?',
          showCancelButton: true,
          confirmButtonText: 'Yes',
          confirmButtonColor: '#bd4444',
          icon: 'warning',
          toast: true,
          width: '100em',
          position: 'top',
          customClass: {
            title: '!text-sm'
          }
        })
        .then(async (result)=> {
          if (result.isConfirmed) {
            await axios.delete(`${employeesURL}/${empnum}`);
            const res = await axios.get(`http://localhost:3001/employees`);
            this.employeesdb = res.data;

            this.$swal({
              titleText:'Employee successfully deleted',
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
          }
      });
        
      } catch (e) {
        console.error(e);
      }
    },
    formattedDate(date){
      let newdate = new Date(date);
      return newdate.toLocaleDateString('en-US', { year: 'numeric', month: 'short', day: 'numeric' })
    }
  }
}
</script>

<template>
  <div class="flex flex-col">
    <UpdateEmployee :employeeid="employeeid" :employee="employee" v-show="showUpdateForm" @show-form="toggleForm" @edit-complete="editComplete" />
    <div class="bg-[#adbdc9] m-[5%] p-[2%] rounded-[20px]">
      <div class="bg-[#1d2b36] p-2.5 flex items-center gap-5 text-white rounded-t-2xl rounded-b-none	-mx-[1.5%] -mt-[1.5%] mb-3">
        <img src="src/assets/employees.svg" alt="Employees Icon" id="employeeicon" class="pl-5 max-w-[80px] max-h-[80px]">
        <h1 class="m-0 text-3xl">Employee Directory</h1>
      </div>
      <div class="flex flex-col">
        <RouterLink 
          to="/newemployee"
          class="self-end bg-[#90cdfc] border-0 rounded p-2.5 mb-2.5 font-bold hover:bg-[#41749c] hover:text-white hover:border-[#6fa7d1] hover:border-0 hover:border-solid"
        >
          Add New Employee
        </RouterLink>
        <table class="employeetable">
          <thead class="border-solid border-4 border-[#a9c5db] text-white bg-[#325a78] rounded-[20px]">
            <td class="font-bold text-center">&nbsp;</td>
            <td class="font-bold text-center">Emp #</td>
            <td class="font-bold text-center">Name</td>
            <td class="font-bold text-center">Gender</td>
            <td class="font-bold text-center">Birth Date</td>
            <td class="font-bold text-center">&nbsp;</td>
            <td class="font-bold text-center">&nbsp;</td>
          </thead>
          <tbody id="employeeslist">
            <tr :key="employee.empNum" v-for="(employee, index) in employeesdb">
              <td class="font-bold text-center">{{++index}}</td>
              <td class="text-center">{{employee.empNum}}</td>
              <td class="text-center">{{employee.firstName}} {{ employee.lastName }}</td>
              <td class="text-center">{{employee.gender}}</td>
              <td class="text-center">{{formattedDate(employee.birthdate)}}</td>
              <td class="text-center">
                <button 
                  @click="editEmployee(employee.id)"
                  class="bg-[#278ddb] border-0 rounded p-1.5 hover:bg-[#1c5a8a] hover:text-white hover:font-bold"
                >Edit</button>
              </td>
              <td class="text-center">
                <button
                  @click="deleteEmployee(employee.id)"
                  class="bg-[#bd4444] border-0 rounded p-1.5 hover:bg-[#9c0e0e] hover:text-white hover:font-bold"
                >Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>
