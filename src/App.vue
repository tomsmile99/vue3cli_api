<template>
    <div>
        <h1> Users List </h1>
        <form ref="reForm" action="#">
            <table>
                <tr>
                    <td>Name : </td>
                    <td><input type="text" v-model="users.name"></td>
                </tr>
                <tr>
                    <td>Username : </td>
                    <td><input type="text" v-model="users.username"></td>
                </tr>
                <!-- <tr>
                    <td>Date Time Add : </td>
                    <td><input type="text" v-model="users.datetime_add"></td>
                </tr> -->
                <tr>
                    <td></td>
                    <td><button type="submit" @click="sendData">Send</button> <button type="reset" @click="clearData">Clear</button></td>
                </tr>
            </table>
        </form>
        <!-- <h3>{{ users }}</h3> -->
    </div>
</template>


<script>

//const axios = require('axios');
import axios from "axios";
import Swal from 'sweetalert2';

axios.defaults.baseURL = 'http://localhost:8080/';

export default {
  data() {
      return {
          users: {
              name: "",
              username: "",
              datetime_add: ""
          }
      }
  },
  methods: {
      getNow: () => {
        const today = new Date();
        const date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
        const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
        const dateTime = date +' '+ time;
        //this.datetime_add = dateTime; 
        return dateTime;
      },
      async sendData(e) {
          e.preventDefault();
            //console.log('SendData active !!!');
            await axios.post('api/users', {
                name: this.users.name,
                username: this.users.username,
                datetime_add: this.getNow()
            })
            .then((response) => {
            /* Read more about handling dismissals below */
               if(response.status === 201){

                   Swal.fire({
                        icon: 'success',
                        title: 'สำเร็จ',
                        text: 'บันทึกข้อมูลสำเร็จ',
                        confirmButtonText : 'ตกลง'
                    })

                    //    console.log(response);
                    //    Swal.fire({
                    //         position: 'top-end',
                    //         icon: 'success',
                    //         title: 'บันทึกข้อมูลสำเร็จ',
                    //         showConfirmButton: false,
                    //         timer: 1500
                    //    })

                    // const Toast = Swal.mixin({
                    //     toast: true,
                    //     position: 'top-end',
                    //     showConfirmButton: false,
                    //     timer: 2000,
                    //     timerProgressBar: true,
                    //     didOpen: (toast) => {
                    //         toast.addEventListener('mouseenter', Swal.stopTimer)
                    //         toast.addEventListener('mouseleave', Swal.resumeTimer)
                    //     }
                    // })
                    // Toast.fire({
                    //     icon: 'success',
                    //     title: 'Save in successfully'
                    // })
                }
            })
            .catch((error) => {
                console.log(error);
            });
         
          // เมื่อเสร็จทุกกระบวนการ
          //this.clearData(); // เรียกใช้ฟังชั่น clearData()
          this.$refs.reForm.reset();
      },
      clearData(){
          this.users.name="";
          this.users.username="";
      }
  }
}
</script>
