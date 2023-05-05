<script setup>
    import { ref } from 'vue'
    import axios from 'axios'
    import { useRoute, useRouter } from 'vue-router'
    import { biList } from '@quasar/extras/bootstrap-icons'

    const router = useRouter()
    
    const logoutUrl = import.meta.env.VITE_API_URL + '/auth/logout'

    axios.get('auth/is-auth')
        .then((response) => {
            const isAuth = !!response.data.data.success;
            !isAuth && router.push(`/auth/login`)
        })
        .catch((error) => {
            console.log(error);
        })

    const leftDrawerOpen = ref(false)

    function toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value
    }
</script>


<template>
    <q-layout view="lHh Lpr lFf" class="bg-white">
        <q-header elevated>
            <q-toolbar>
                <q-btn
                    flat
                    dense
                    round
                    @click="toggleLeftDrawer"
                    :icon="biList"
                />
                <q-toolbar-title>HR PLUS</q-toolbar-title>

                <q-btn-dropdown stretch flat label="Dropdown" :dropdown-icon="biList">
                    <q-list>
                    <q-item-label header>Folders</q-item-label>
                    <q-item v-for="n in 3" :key="`x.${n}`" clickable v-close-popup tabindex="0">
                        <q-item-section avatar>
                        <q-avatar icon="folder" color="secondary" text-color="white" />
                        </q-item-section>
                        <q-item-section>
                        <q-item-label>Photos</q-item-label>
                        <q-item-label caption>February 22, 2016</q-item-label>
                        </q-item-section>
                        <q-item-section side>
                        <!-- <q-icon name="info" /> -->
                        </q-item-section>
                    </q-item>
                    <q-separator inset spaced />
                    <q-item-label header>Files</q-item-label>
                    <q-item v-for="n in 3" :key="`y.${n}`" clickable v-close-popup tabindex="0">
                        <q-item-section avatar>
                        <q-avatar icon="assignment" color="primary" text-color="white" />
                        </q-item-section>
                        <q-item-section>
                        <q-item-label>Vacation</q-item-label>
                        <q-item-label caption>February 22, 2016</q-item-label>
                        </q-item-section>
                        <q-item-section side>
                        <!-- <q-icon name="info" /> -->
                        </q-item-section>
                    </q-item>
                    </q-list>
                </q-btn-dropdown>


            </q-toolbar>
            
        </q-header>
    
        <q-drawer
            v-model="leftDrawerOpen"
            show-if-above
            bordered
        >
            <q-list>
                <q-item to="/account/companies">
                    <q-item-section>
                        <q-item-label>Все организации</q-item-label>
                    </q-item-section>
                </q-item>
                <q-item to="/account/company/new">
                    <q-item-section>
                        <q-item-label>Добавить новую организацию</q-item-label>
                    </q-item-section>
                </q-item>
                <q-item :href="logoutUrl">
                    <q-item-section>
                        <q-item-label>Выйти</q-item-label>
                    </q-item-section>
                </q-item>
            </q-list>
        </q-drawer>

        <q-page-container class="bg-grey-1">
            <div class="acc-subheader bg-primary text-white">
                <h1 class="text-h5 text-weight-medium">Добавить организацию</h1>
            </div>

            <div class="acc-main">
                <slot></slot>
            </div>
        </q-page-container>
    </q-layout>
</template>


<style lang="scss">
    .q-layout__shadow:after {
        box-shadow: none;
    }

    .acc-subheader {
        padding: 2rem 18px 6rem;

        h1 {
            margin: 0;
        }
    }


    .acc-main {
        position: relative;
        margin: -45px 1.5rem 0;
    }

    .box {
        position: relative;
        padding: 1.25rem;
        background-color: #fff;
        border-radius: 0.5rem;
        box-shadow: 0 0.75rem 1.5rem rgba(0, 0, 0, 0.03);
    }
</style>