<script setup>
    import { ref, onMounted, watch } from 'vue'
    import { useRoute, useRouter } from 'vue-router'
    import AccountBase from './AccountBase.vue'
    import axios from 'axios'
    import { useQuasar } from 'quasar'

    const route = useRoute()
    const router = useRouter()
    const $q = useQuasar()

    const form = ref({
        description: ''
    })
    const id = ref(0)
    
    const send = function() {
        axios.post(`company/update`, form.value, {
            headers: {
                "Content-Type": "multipart/form-data",
            },
        })
            .then((response) => {
                $q.notify({ message: 'Данные обновлены', color: 'positive' })
            })
            .catch((error) => {
                console.log(error);
            })
    }

    onMounted(async () => {
        console.log('mounted')
        await router.isReady()
        id.value = route.params.id || 0

        axios.get(`company/${id.value}`)
            .then((response) => {
                if (response.data.data.success) {
                    form.value = response.data.data.data
                    form.value.description = form.value.description || ''
                } else {
                    alert('Компания не найдена')
                }
            })
            .catch((error) => {
                console.log(error);
            })
    })

    watch(
        () => route.params.tab, async tab => {
            console.log(tab)
        }
    )

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
        <div class="row">
            <div class="col-2 q-px-lg q-gutter-md">
                <div class="box">
                    <router-link :to="`/account/company/${id}/edit`">111</router-link>
                </div>
                <div class="box">
                    <router-link :to="`/account/company/${id}/logo`">111</router-link>
                </div>
                <div class="box">
                    <router-link :to="`/account/company/${id}/social`">111</router-link>
                </div>
            </div>

            <div class="col-10">
                <q-form @submit.prevent="send()" class="box" action="">
                    <h5>Введите общие данные организации:</h5>

                    <div class="row q-col-gutter-lg">
                        <template v-for="i in fields">
                            <div class="col-6">
                                <q-input v-model="form[i.name]" outlined :name="i.name" :type="i.type || 'text'" :label="i.label" :required="i.required"/>
                            </div>
                        </template>

                        <div class="col-12">
                            <p>Описание организации</p>
                            <q-editor v-model="form.description" min-height="5rem" />
                        </div>

                        <div class="col-12">
                            <q-btn label="Сохранить" type="submit" color="primary"/>
                        </div>

                        <div class="col-12">
                            <q-file
                                name="logo"
                                v-model="form.logo"
                                outlined
                                label="Загрузить логотип"
                            />
                        </div>
                    </div>
                </q-form>
            </div>
        </div>
        
    </account-base>
</template>