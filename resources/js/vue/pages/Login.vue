<template>

    <div>
        <div class="container-fluid d-flex justify-content-center h-100" style="margin-top: 10%">
            <div class="card shadow p-0 mb-3  rounded" style="width: 18rem;">
                <div class=" card-body d-flex justify-content-center form_container">
                    <form  method="post" >
                        <div class="input-group mb-3">
                            <div class="input-group-prepend">
                                <span class="input-group-text bg-success text-light">
                                    <i class="material-icons">email</i></span>
                            </div>
                            <input type="email" class="form-control" id="inputEmail" aria-describedby="emailHelp"
                                v-model="user.email"
                                placeholder="Email">
                        </div>
                        <div class="input-group mb-2">
                            <div class="input-group-prepend">
                                <span class="input-group-text bg-success text-light">
                                    <i class="material-icons">vpn_key</i></span>
                            </div>
                            <input type="password" name="password" class="form-control" id="inputPassword"
                                v-model="user.password"
                                placeholder="senha">
                        </div>

                        <div class="d-flex justify-content-center mt-3 ">
                            <button type="submit" class="btn btn-success "
                            @click="login"
                            style="width: 100%;">Entrar </button>
                        </div>
                    </form>

                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return {
            user: {
                email: '',
                password: '',
            },
            errors: []
        }
    },
    methods: {
        login(e){
            e.preventDefault()
            axios.get('/sanctum/csrf-cookie').then(response =>{
               axios.post('api/login', {
                    email: this.user.email,
                    password: this.user.password
                })
                .then(response => {
                    console.log(response.data);
                    const data = new Date();
                    localStorage.setItem('toDay', data.getTime())
                    localStorage.setItem('isLoggedIn', true)
                    localStorage.setItem('user', response.data.user)
                    localStorage.setItem('userId', response.data.userId)
                    if (response.data.success) {
                        this.emitter.emit('isLoggedIn', true)
                        this.$router.push('/')
                    }
                })
                .catch(function (error) {
                    //console.error(error.data);
                    alert("ERRO!!! E-mail ou senha inválidos");

                });
            })
        }
    },
}
</script>

<style>
</style>
