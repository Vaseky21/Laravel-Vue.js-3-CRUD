<template>
  <div class="container text-center">
    <div class="row align-items-start">
    <div class="col">
      <h2>Registrace Klienta</h2>
      <form @submit.prevent="save">
      <div class="container col" >
        <div class="form-group">
          <label>Jméno a příjmení</label>
          <input type="text" v-model="employee.name" class="form-control"  placeholder="Jméno a příjmení">
        
        </div>
        <div class="form-group">
          <label>Adresa</label>
          <input type="text" v-model="employee.address" class="form-control"  placeholder="Adresa">
        
        </div>

        <div class="form-group">
          <label>Telefon</label>
          <input type="text" v-model="employee.mobile" class="form-control"  placeholder="Telfon">
        
        </div>
      </div>
      <button type="submit" class="btn btn-primary" style="margin: 20px;">Uložit</button>
      </form>
    </div>
    <div class="col-8">
      <h2>Seznam </h2>
        <table class="table table-dark">
          <thead>
            <tr>
          
              <th scope="col">Jméno a příjmení</th>
              <th scope="col">Adresa</th>
              <th scope="col">Telefon</th>
              <th scope="col">AKCE</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="employee in result" v-bind:key="employee.id">
                  
                  
                  <td>{{ employee.name }}</td>
                  <td>{{ employee.address }}</td>
                  <td>{{ employee.mobile }}</td>
                  <td>
                    <button type="button" class="btn btn-warning" @click="edit(employee)">Edit</button>
                    <button type="button" class="btn btn-danger"  @click="remove(employee)">Delete</button>
                  </td>
                </tr>
            
          </tbody>
  
        </table>
    </div>
  </div>
  </div>

  </template>
  
  <script>
    import Vue from 'vue';
    import axios from 'axios';

     Vue.use(axios)


  export default {
    name: 'EmployeeView',
    data () {
      return {
        result: {},
        employee:{
                   id: '',
                   name: '',
                   address: '',
                   mobile: ''


        }
      }
    },
    created() { 
        this.EmployeeLoad();
    },
    mounted() {
          console.log("mounted() called.......");
         
      },

    methods: {
           EmployeeLoad()
           {
                 var page = "http://127.0.0.1:8000/api/employees";
                 axios.get(page)
                  .then(
                      ({data})=>{
                        console.log(data);
                        this.result = data;
                        this.employee.name = '';
                        this.employee.address = '',
                        this.employee.mobile = ''
                        this.id = ''
                      }
                  );
           },
           
          
           save()
           {
            if(this.employee.id == '')
              {
                this.saveData();
                this.employee.name = '';
                this.employee.address = '',
                this.employee.mobile = ''
                this.id = ''
              }
              else
              {
                this.updateData();
                this.employee.name = '';
                this.employee.address = '',
                this.employee.mobile = ''
                this.id = ''
              }       

           },
           saveData()
           {
            axios.post("http://127.0.0.1:8000/api/save", this.employee)
            .then(
              ({data})=>{
                alert("saveddddd");
                this.EmployeeLoad();
                this.employee.name = '';
                this.employee.address = '',
                this.employee.mobile = ''
                this.id = ''
              }
            )

           },
           edit(employee)
           {
            this.employee = employee;
           
           },
           updateData()
           {
              var editrecords = 'http://127.0.0.1:8000/api/update/'+ this.employee.id;
              axios.put(editrecords, this.employee)
              .then(
                ({data})=>{
                 
                  alert("Updated!!!");
                  this.employee.name = '';
                  this.employee.address = '',
                  this.employee.mobile = ''
                  this.id = ''
                  this.EmployeeLoad();
                }
              );

           },

           remove(employee){

             var url = `http://127.0.0.1:8000/api/delete/${employee.id}`;



             // var url = 'http://127.0.0.1:8000/api/delete/'+ employee.id;
              axios.delete(url);
              alert("Deleteddd");
              this.EmployeeLoad();
            }
      }
  }
  </script>
<style>
.col-8{
  background-color: rgb(86, 86, 139);
}
.col{
  background-color: rgb(68, 146, 98);
}
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;

}
.text-center{
  background-color:rgb(86, 86, 139);
}


</style>