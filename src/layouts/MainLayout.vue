<template>
    <q-layout view="hHh lpR fFf">
        <q-resize-observer @resize="handleResize"></q-resize-observer>
        <q-page-container>
            <div class="fixed-top-left" style="top: 0px; left: 20px; height: 5rem">
                <img src="assets/logo.png" style="height: 5rem" />
            </div>
            <div>
                <router-view />
            </div>
            <q-page-sticky position="bottom-right" :offset="[18, 18]">
                <q-btn size="26px" round flat> <img src="assets/upload.png" style="height: 40px" /></q-btn>
            </q-page-sticky>
        </q-page-container>
    </q-layout>
</template>

<script>
import { defineComponent, ref } from 'vue';
import { useStore } from 'vuex';

export default defineComponent({
    name: 'MainLayout',
    setup() {
        const $store = useStore();

        const leftDrawerOpen = ref(false);
        const rightDrawerOpen = ref(false);

        const handleResize = function (size) {
            $store.commit('comm/setPageSize', size);
        };

        return {
            leftDrawerOpen,
            toggleLeftDrawer() {
                leftDrawerOpen.value = !leftDrawerOpen.value;
            },

            rightDrawerOpen,
            toggleRightDrawer() {
                rightDrawerOpen.value = !rightDrawerOpen.value;
            },

            handleResize,
        };
    },
});
</script>
