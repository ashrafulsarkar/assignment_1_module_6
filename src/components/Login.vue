<script setup>
import { ref, reactive } from 'vue';
const login = ref(true)
const isLogin = ref(false)
const massage = ref('')
const data = reactive({
    email: '',
    password: '',
    cpassword: ''
})

function loginform() {
    const localData = localStorage.getItem('registationData')? JSON.parse(localStorage.getItem('registationData')) : ''

    if ('' == localData) {
        massage.value = 'Register Your Account First.'
    }else if (data.email == localData.email  && data.password == localData.password) {
        massage.value = ''
        isLogin.value = true
    } else {
        massage.value = 'Email or Password is not match.'
    }
}


function registation() {
    if ( ''==data.email || '' == data.password || '' == data.cpassword) {
        massage.value = 'Please fillup all input.'
    }else if ('' == massage.value) {
        localStorage.setItem('registationData', JSON.stringify(data))
        data.email = ''
        data.password = ''
        data.cpassword = ''
        massage.value = 'Registation Successfully'
    }
}

function mailCheck() {
    if (/^[^@]+@\w+(\.\w+)+\w$/.test(data.email) == false) {
        massage.value = 'Please enter a valid email address';
    } else {
        massage.value = ''
    }
}

function passworkCheck() {
    if (data.password != data.cpassword) {
        massage.value = 'Password & Confirm Password are not match!'
    } else {
        massage.value = ''
    }
}
function loginReg() {
    login.value = !login.value
    massage.value = ''
    data.email = ''
    data.password = ''
    data.cpassword = ''
}
</script>
<template>
    <div class="mt-6">
        <div v-show="!isLogin" class="max-w-lg mx-auto">
            <h2 class="text-center font-bold text-3xl mb-3">{{ login?'Login':'Registation' }} Your Account</h2>
            <label class="block">
                <span class="after:content-['*'] after:ml-0.5 after:text-red-500 block text-sm font-medium text-slate-700">
                    Email
                </span>
                <input type="email" @input="mailCheck()" v-model="data.email" class="mt-1 px-3 py-2 bg-white border shadow-sm border-slate-300 placeholder-slate-400 focus:outline-none focus:border-sky-500 focus:ring-sky-500 block w-full rounded-md sm:text-sm focus:ring-1" placeholder="you@example.com" />
            </label>
            <label class="block">
                <span class="after:content-['*'] after:ml-0.5 after:text-red-500 block text-sm font-medium text-slate-700">
                    Password
                </span>
                <input type="password" v-model="data.password" class="mt-1 px-3 py-2 bg-white border shadow-sm border-slate-300 placeholder-slate-400 focus:outline-none focus:border-sky-500 focus:ring-sky-500 block w-full rounded-md sm:text-sm focus:ring-1" />
            </label>
            <label v-show="!login" class="block">
                <span class="after:content-['*'] after:ml-0.5 after:text-red-500 block text-sm font-medium text-slate-700">
                    Confirm Password
                </span>
                <input type="password" @input="passworkCheck()" v-model="data.cpassword" class="mt-1 px-3 py-2 bg-white border shadow-sm border-slate-300 placeholder-slate-400 focus:outline-none focus:border-sky-500 focus:ring-sky-500 block w-full rounded-md sm:text-sm focus:ring-1" />
            </label>
            <input v-show="login" @click="loginform()" type="button" value="Login" class="mt-2 px-3 py-2 cursor-pointer text-white bg-gray-700 border shadow-sm border-slate-300 placeholder-slate-400 focus:outline-none focus:border-sky-500 focus:ring-sky-500 block w-full rounded-md sm:text-sm focus:ring-1" />
            <input v-show="!login" @click="registation()" type="button" value="Register" class="mt-2 px-3 py-2 cursor-pointer text-white bg-gray-700 border shadow-sm border-slate-300 placeholder-slate-400 focus:outline-none focus:border-sky-500 focus:ring-sky-500 block w-full rounded-md sm:text-sm focus:ring-1" />
            <div class="text-center mt-2">
                <input type="button" @click="loginReg()" :value="login?'Registation':'Login'" class="cursor-pointer text-blue-800" />
            </div>
            <p class="text-center mt-2 mb-2">{{ massage }}</p>
        </div>
        <div v-show="isLogin" class="max-w-lg mx-auto">
            <h1 class="text-center font-bold text-3xl">Welcome page</h1>
        </div>
    </div>
</template>