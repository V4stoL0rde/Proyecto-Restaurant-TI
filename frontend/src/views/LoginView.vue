<template>

  <HederViewVue></HederViewVue>
  <section class="vh-100">
  <div class="container-fluid">
    <div class="row">
      <div class="col-sm-6 text-black">

        <div class="d-flex align-items-center px-5 ms-xl-4">
          <img src="../assets/logo-h.png" alt="300" width="350" >
         
        </div>

        <div class="d-flex align-items-center h-custom-2 px-5 ms-xl-4 mt-5 pt-5 pt-xl-0 mt-xl-n5">

          <form style="width: 23rem;">

            <h3 class="fw-normal mb-3 pb-3" style="letter-spacing: 1px;">Log in</h3>

            <div class="form-outline mb-4">
              <input type="input" id="form2Example18" class="form-control form-control-lg" v-model="nombre" />
              <label class="form-label" for="form2Example18">Username</label>
            </div>

            <div class="form-outline mb-4">
              <input type="password" id="form2Example28" class="form-control form-control-lg" required v-model="pass" />
              <label class="form-label" for="form2Example28">Password</label>
            </div>

            <div class="pt-1 mb-4">
              <button  @click="login()" class="btn btn-info btn-lg btn-block" type="button">Login</button>
            </div>

            <p class="small mb-5 pb-lg-2"><a class="text-muted" href="/forgot">Forgot password?</a></p>
            <p>Don't have an account? <a href="/register" class="link-info">Register here</a></p>

          </form>

        </div>

      </div>
      <div class="col-sm-6 px-0 d-none d-sm-block">
        <img src="../assets/login.jpg"
          alt="Login image" class="w-100 vh-100" style="object-fit: cover; object-position: left;">
      </div>
    </div>
  </div>
</section>
</template>

<script>

import axios from 'axios'
import HederViewVue from '@/components/HederView.vue'

export default {
  name: 'LoginView',

  components:{
  HederViewVue
},
  data: function(){
    return {
      nombre: "",
      pass: "",
      error: false,
      error_msg: "",
      
    }
  },
  methods:{
    login(){
        let json={
           "Username": this.nombre,
           "password":this.pass
        }
        
        axios.post('http://192.168.137.97:3000/api/users/autenticate', json)
  .then(response => {
    const { token, Username, role } = response.data;

    const userRole = Array.isArray(role) ? role[0] : role; 

    console.log(userRole);

    // Guardar datos en localStorage
    localStorage.setItem("token", token);
    localStorage.setItem("Username", Username);
    localStorage.setItem("role", userRole);

    // Redireccionar según el rol
    switch (userRole) {
      case "ADMIN":
        this.$router.push("/");
        break;
      case "TRABAJADOR":
      this.$router.push("/");
      break;
      case "CLIENTE":
        this.$router.push("/");
        break;
      default:
        alert("Rol no reconocido");
    }
  })
  .catch(error => {
    console.error('Error al confirmar:', error);
    alert("Ocurrió un error: " + (error.response?.data?.message || error.message));
  });
        
    }
  }
}

</script>


<style>
.wrapper {
  display: flex;
  align-items: center;
  flex-direction: column; 
  justify-content: center;
  width: 100%;
  min-height: 100%;
  padding: 40px;
}
</style>