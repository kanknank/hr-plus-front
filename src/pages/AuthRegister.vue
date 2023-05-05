<script setup>
    import { ref } from 'vue'
    import axios from 'axios'
    import AuthBase from './AuthBase.vue'
    import AppInputPassword from '../components/AppInputPassword.vue'

    const form = ref({})
    const errors = ref({})
    const success = ref(false)
    const successMessage = ref('')

    const send = function() {
        axios.post('auth/register', form.value)
            .then((response) => {
                errors.value = response.data.data.errors || {}
                success.value = response.data.data.success
                successMessage.value = response.data.data.message || ''
            })
            .catch((error) => {
                console.log(error);
            })
    }
</script>

<template>
    <auth-base title="Регистрация">
        <form @submit.prevent="send()" class="q-gutter-md" action="">
            <template v-if="!success">
                <q-input v-model="form.email" outlined name="email" type="email" label="Email"
                    :error="!!errors.email || null"
                    :error-message="errors.email"
                />
                <app-input-password v-model="form.specifiedpassword"
                    :error="!!errors.specifiedpassword || null"
                    :error-message="errors.specifiedpassword"
                />
                <app-input-password v-model="form.confirmpassword"
                    :error="!!errors.confirmpassword || null"
                    :error-message="errors.confirmpassword"
                    label="Подтвердите пароль"
                    name="confirmpassword"
                />
                <div>
                    <q-btn label="Зарегистрироваться" color="primary" class="full-width" type="submit"/>
                </div>

                <div class="text-center">
                    <router-link class="text-primary" to="/auth/login">Войти</router-link>
                </div>
            </template>

            <template v-else>
                <p class="text-body1">{{ successMessage }}</p>

                <div class="text-center">
                    <q-btn label="Войти" to="/auth/login" color="primary" class="full-width"/>
                </div>
            </template>
        </form>
    </auth-base>
</template>