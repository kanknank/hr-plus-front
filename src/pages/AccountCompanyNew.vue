<script setup>
    import { ref, onMounted } from 'vue'
    import { useRoute, useRouter } from 'vue-router'
    import AccountBase from './AccountBase.vue'
    import axios from 'axios'
    import { useQuasar } from 'quasar'

    const router = useRouter()
    const $q = useQuasar()

    const form = ref({})
    
    const send = function() {
        axios.post(`company/new`, form.value)
            .then((response) => {
                $q.notify({ message: 'Организация создана', color: 'positive' })
                router.push(`/account/companies`)
            })
            .catch((error) => {
                console.log(error);
            })
    }

    const fields = [
        { name: 'name', label: 'Наименование организации', required: true },
        { name: 'brand', label: 'Бренд' },
        { name: 'address', label: 'Адрес организации' },
        { name: 'phone', label: 'Рабочий телефон', type: 'tel' },
        { name: 'site', label: 'Сайт организации' },
        { name: 'email', label: 'E-mail организации', type: 'email' },
        { name: 'inn', label: 'ИНН' },
        { name: 'ogrn', label: 'ОГРН' }
    ]
</script>

<template>
    <account-base>
        <form @submit.prevent="send()" class="q-gutter-md" action="">
            <h5>Введите общие данные организации:</h5>

            <div class="row">
                <template v-for="i in fields">
                    <div class="col-6 q-px-md q-py-md">
                        <q-input v-model="form[i.name]" outlined :name="i.name" :type="i.type || 'text'" :label="i.label" :required="i.required"/>
                    </div>
                </template>
            </div>

            <div>
                <q-editor v-model="form.description" min-height="5rem" />
            </div>

            <q-btn label="Submit" type="submit" color="primary"/>
        </form>
    </account-base>
</template>