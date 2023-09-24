<script setup lang="ts">
import Joi from 'joi';
import { ref } from 'vue';

import InputLogin from '@/components/inputs/InputLogin.vue';
import ValidateMessage from '@/components/validates/ValidateMessage.vue';
import ValidateLogin from '@/validations/ValidateLogin';
import { newValidateObj, initValidateObj } from '@/validations/ValidateCommon';

import LogoText from '@/components/logo/LogoText.vue';
import ButtonBig from '@/components/buttons/ButtonBig.vue';

const schema = Joi.object({
    isErrorLoginId: Joi.string().required(),
    isErrorLoginPw: Joi.string().required()
});
let validateObj = ref(newValidateObj({
    isErrorLoginId: false,
    isErrorLoginPw: false
}));
const validate = ()=>{
    return schema.validate({isErrorLoginId: loginId.value, isErrorLoginPw: loginPw.value}, {abortEarly: false});
}
let loginId = ref<string | undefined>();
let loginPw = ref<string | undefined>();
const login = ()=>{
    initValidateObj(validateObj.value);

    const validateResult = validate();
    if(validateResult.error){
        validateResult.error.details.forEach(element => {
            if(element?.context?.key){
                validateObj.value[element.context.key] = true;
            }
        });
    }
    else{
        // 성공 api 날리기
    }
}
</script>

<template>
    <main>
        <div class="login-wrapper">
            <LogoText />
            
            <div class="login-box">
                <h2 class="login-title">로그인</h2>
                <h4 class="login-sub-title">아이디와 비밀번호를 입력하세요.</h4>
                <form>
                    <div class="form-row login-id">
                        <InputLogin :type="'text'" :placeholder="'아이디'" v-model="loginId"/>
                        <ValidateMessage v-if="validateObj?.isErrorLoginId" :error-msg="ValidateLogin.loginId"/>
                    </div>
                    <div class="form-row login-pw">
                        <InputLogin :type="'password'" :placeholder="'비밀번호'" v-model="loginPw"/>
                        <ValidateMessage v-if="validateObj?.isErrorLoginPw" :error-msg="ValidateLogin.loginPw"/>
                    </div>
                </form>
                <div class="memorize-id">
                    <input type="checkbox" id="memorize-id">
                    <label for="memorize-id">아이디 저장</label>
                </div>
                <ButtonBig @click="login">로그인</ButtonBig>
            </div>
            <span class="create-id"><router-link :to="{path: '/sign-up/user-name'}">계정 만들기</router-link></span>
        </div>
    </main>
</template>

<style scoped>
main {
    margin: 0 auto;
    width: 50%;
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    color: var(--main-black);
}
.logo {
    text-align: center;
}
.login-title, .login-sub-title {
    text-align: center;
}
.login-sub-title {
    margin-top: 14px;
}
.login-box {
    margin: 23px 0;
    padding: 51px;
    display: flex;
    flex-direction: column;
    box-sizing: border-box;
    width: 450px;
    height: auto;
    min-height: 500px;
    border: 1px solid #CACACA;
    border-radius: 5px;
}
.login-box form {
    margin-top: 32px;
    display: flex;
    flex-direction: column;
    width: -webkit-fill-available;
    gap: 23px;
}
.form-row {
    position: relative;
}
.button-big {
    padding: 13px 15px;
    width: -webkit-fill-available;
    height: 52px;
    font-size: 17px;
    border-radius: 5px;
}
.memorize-id input {
    border: 1px solid var(--main-gray-3);
}
.button-big {
    margin-top: 45px;
    border: 0;
    background-color: var(--main);
    color: var(--main-white);
}
.memorize-id {
    margin-top: 13px;
    display: flex;
    align-items: center;
}
.memorize-id input[type="checkbox"] {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    border-radius: 3px;
    cursor: pointer;
    height: 16px;
    outline: 0;
    width: 16px;
}
.memorize-id input[type="checkbox"]::after {
    border: solid #fff;
    border-width: 0 2px 2px 0;
    content: '';
    display: none;
    height: 60%;
    left: 37%;
    position: relative;
    top: 10%;
    transform: rotate(45deg);
    width: 35%;
}
.memorize-id input[type="checkbox"]:checked {
    background: var(--main-success);
}
.memorize-id input[type="checkbox"]:checked::after {
    display: block;
}
.memorize-id label {
    margin-left: 9px;
    color: var(--main-black-soft);
}
.create-id {
    color: var(--main-success);
    font-size: 14px;
    font-weight: 500;
}
</style>