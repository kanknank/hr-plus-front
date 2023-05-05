<script setup>
    import { ref } from 'vue'
    import AccountBase from './AccountBase.vue'
    import axios from 'axios'

    const rows = ref([])

    axios.post('company/list')
        .then((response) => {
            rows.value = response.data.data.data
            console.log(response.data.data.data);
        })
        .catch((error) => {
            console.log(error);
        })

    const columns = [
        { name: 'name', label: 'Наименование организации', field: 'name' },
        { name: 'inn', label: 'ИНН', field: 'inn' },
        // { name: 'carbs', label: 'Вакансии', field: 'carbs' },
        // { name: 'protein', label: 'Пользователи', field: 'protein' },
        // { name: 'sodium', label: 'Отклики в работе', field: 'sodium' },
        // { name: 'calcium', label: 'Действие', field: 'calcium' },
        { name: 'user', label: 'Создатель аккаунта', field: 'user' },
        { name: 'createdon', label: 'Дата создания', field: 'createdon' },
    ]
</script>

<template>
    <account-base>
        <q-table
            :rows="rows"
            :columns="columns"
            row-key="name"
            hide-bottom
        >
            <template v-slot:body-cell-name="props">
                <q-td :props="props">
                    <router-link :to="`/account/company/${props.row.id}`">{{ props.row.name }}</router-link>
                </q-td>
            </template>
        </q-table>
    </account-base>
</template>

