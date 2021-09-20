<template>
    <q-layout view="hHh lpR fFf">
        <q-header class="bg-black text-white" style="height: 100px">
            <q-toolbar>
                <!-- <q-btn dense flat round icon="menu" @click="toggleLeftDrawer" /> -->
                <q-toolbar-title>
                    <!-- <q-icon name="img:./assets/logo.png" /> -->
                    <!-- <q-icon name="img:./assets/logo_sub.png" /> -->
                    <div style="height: 100px; position: absolute; top: 0px; left: 30px">
                        <img src="assets/logo.png" style="height: 100px; position: absolute; top: 0px; left: 0px" />
                        <img src="assets/logo_sub.png" style="height: 100px; position: absolute; top: 0px; left: 40px" />
                    </div>
                    <!-- <q-avatar>
                        <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg" />
                    </q-avatar> -->
                </q-toolbar-title>

                <!-- <q-btn dense flat round icon="menu" @click="toggleRightDrawer" /> -->
            </q-toolbar>
        </q-header>

        <q-drawer v-model="leftDrawerOpen" side="left" bordered>
            <!-- drawer content -->
        </q-drawer>

        <q-drawer v-model="rightDrawerOpen" side="right" bordered>
            <!-- drawer content -->
        </q-drawer>

        <q-page-container class="bg-white">
            <router-view />
            <q-page-sticky position="bottom-right" :offset="[18, 18]">
                <q-btn size="35px" round flat> <img src="assets/upload.png" style="height: 50px" /></q-btn>
            </q-page-sticky>
        </q-page-container>
    </q-layout>
</template>

<script>
import { defineComponent, ref } from 'vue';

export default defineComponent({
    name: 'MainLayout',
    setup() {
        const leftDrawerOpen = ref(false);
        const rightDrawerOpen = ref(false);

        return {
            leftDrawerOpen,
            toggleLeftDrawer() {
                leftDrawerOpen.value = !leftDrawerOpen.value;
            },

            rightDrawerOpen,
            toggleRightDrawer() {
                rightDrawerOpen.value = !rightDrawerOpen.value;
            },
        };
    },
});
</script>
