<template>
      <HederViewVue></HederViewVue>
  <section class="vh-100">
    <div class="container-fluid h-100">
      <div class="row h-100">

        <!-- Columna izquierda: Login -->
        <div class="col-sm-6 text-black d-flex flex-column justify-content-center align-items-center">

          <!-- Logo -->
          <div class="mb-4">
            <img src="../assets/logo-h.png" alt="Logo" width="350" />
          </div>

          <!-- Formulario -->
          <form style="width: 23rem;">
            <h3 class="fw-normal mb-3 pb-3" style="letter-spacing: 1px;">Log in</h3>

            <div v-if="logged">
              <div class="form-outline mb-4">
                <input type="input" class="form-control form-control-lg" v-model="nombre" />
                <label class="form-label">Username</label>
              </div>

              <div class="form-outline mb-4">
                <input type="input" class="form-control form-control-lg" v-model="correo" />
                <label class="form-label">Correo</label>
              </div>

              <div class="form-outline mb-4">
                <select class="form-control form-control-lg" v-model="role">
                  <option value="CLIENTE">CLIENTE</option>
                  <option value="TRABAJADOR">TRABAJADOR</option>
                </select>
                <label class="form-label">Tipo de Usuario</label>
              </div>

              <div class="form-outline mb-4">
                <input type="password" class="form-control form-control-lg" v-model="nuevaContrasena" />
                <label class="form-label">Nueva contraseña</label>
              </div>

              <div class="form-outline mb-4">
                <input type="password" class="form-control form-control-lg" v-model="confirmarContrasena" />
                <label class="form-label">Confirmar nueva contraseña</label>
              </div>

              <div class="pt-1 mb-4">
                <button @click="login()" class="btn btn-info btn-lg btn-block" type="button">Enviar</button>
              </div>
            </div>

            <div v-if="loggedIn">
              <div class="form-outline mb-4">
                <input type="input" class="form-control form-control-lg" v-model="codigo" />
                <label class="form-label">Código</label>
              </div>

              <div class="pt-1 mb-4">
                <button @click="confirmar()" class="btn btn-info btn-lg btn-block" type="button">Confirmar</button>
              </div>
            </div>
          </form>
        </div>

        <!-- Columna derecha: Imagen -->
        <div class="col-sm-6 px-0 d-none d-sm-block">
          <img
            src="../assets/login.jpg"
            alt="Login image"
            class="  d-block"
            style="object-fit: cover; object-position: left;"
          />
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
        nombre: '',
        correo:'',
      loggedIn: false,
      logged: true,
      codigo: '',
      nuevaContrasena: '',
      confirmarContrasena: '',
      role: "CLIENTE"
        
      }
    },
    methods:{
      login(){
        if (this.nuevaContrasena == this.confirmarContrasena) {
          axios.post('http://192.168.137.97:3000/api/users/nuevo-usuario', {Username:this.nombre,correo:this.correo,password:this.confirmarContrasena,role:this.role})
          .then( data =>{
            console.log(data.data)
            this.loggedIn=true;
            this.logged=false;
          })
          
        }
        else{
            alert("las contraseñas no son iguales")
        }
        
         
      },
      confirmar() {
            axios.post('http://192.168.137.97:3000/api/users/nuevo', {Username:this.nombre,code:this.codigo})
          .then( data =>{
            console.log(data.data)

            if (data.status==200) {
              alert("bienvenido");
              this.$router.push("/");
            }
            
           
          }
        )
        .catch(error => {
        console.error('Error al confirmar:', error);
        alert("Ocurrió un error",+ (error.response?.data?.message || error.message));
  });
        }
    }
  }
  
  </script>
  
  
  <style>
 html, body {
  height: 100%;
  margin: 0;
  padding: 0;
}
  </style>