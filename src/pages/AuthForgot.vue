<script setup>
    import { ref } from 'vue'
    import axios from 'axios'
    import AuthBase from './AuthBase.vue'

    const form = ref({})
    const error = ref('')
    const success = ref(false)
    const successMessage = ref('')

    const send = function() {
        axios.post('auth/forgot', form.value)
            .then((response) => {
                if (response.data.data.success) {
                    success.value = true
                    successMessage.value = response.data.data.message || ''
                } else {
                    error.value = response.data.data.message || ''
                }
            })
            .catch((error) => {
                console.log(error);
            })
    }
</script>

<template>
    <auth-base title="Забыли пароль?">
        <form @submit.prevent="send()" class="q-gutter-md" action="">
            <template v-if="!success">
                <q-input v-model="form.email" outlined name="email" type="email" label="Email"
                    :error="!!error || null" :error-message="error"
                />

                <div>
                    <q-btn label="Сбросить пароль" color="primary" class="full-width" type="submit"/>
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