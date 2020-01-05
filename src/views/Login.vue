<template>
    <div>
        <el-form :rules="rules" :model="loginForm"  ref="loginForm" class="loginContainer">
            <h3 class="loginTitle">系统登录</h3>
            <el-form-item prop="username">
                <el-input type="text" v-model="loginForm.username" auto-complete="off" placeholder="请输入用户名"></el-input>
            </el-form-item>
            <el-form-item prop="password">
                <el-input type="password" v-model="loginForm.password" auto-complete="off"
                          placeholder="请输入密码"></el-input>
            </el-form-item>
            <el-checkbox v-model="check" class="loginRem"></el-checkbox>
            <el-button type="primary" style="width: 100%" @click="submitLogin">登录</el-button>
        </el-form>
    </div>
</template>

<script>
    export default {
        name: "Login",
        data() {
            return {
                loginForm: {
                    username: "admin",
                    password: "123"
                },
                check: false,
                rules: {
                    username: [{required: true, message: "请输入用户名", triangle: 'blur'}],
                    password: [{required: true, message: "请输入密码", triangle: 'blur'}]
                }
            }
        },
        methods:{
            submitLogin() {
                this.$refs.loginForm.validate((valid) => {
                    if (valid) {
                        // alert('submit!');
                        this.postKeyValueRequest('/doLogin', this.loginForm).then(resp => {
                            if (resp) {
                                // alert(JSON.stringify(resp));
                                window.sessionStorage.setItem("user",JSON.stringify(resp.obj));
                                this.$router.replace('/home');//replace 不可后退历史页面  push 开会退页面
                            }
                        });
                    } else {
                        this.$message.error('请完善信息');
                        return false;
                    }
                });
            }
        }
    }
</script>

<style>
    .loginContainer {
        border-radius: 15px;
        margin: 180px auto;
        background-clip: padding-box;
        width: 350px;
        padding: 35px 35px 15px 35px;
        background: #fff;
        border: 1px solid #eaeaea;
        box-shadow: 0  0 5px #cac6c6;
    }

    .loginTitle {
        margin: 15px auto 20px auto;
        text-align: center;
        color: #505458;
    }

    .loginRem {
        text-align: left;
        margin: 0 0 15px 0;
    }
</style>