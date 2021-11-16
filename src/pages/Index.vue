<template>
    <q-page class="flex flex-center">
        <!-- <img alt="Quasar logo" src="~assets/quasar-logo-vertical.svg" style="width: 200px; height: 200px" /> -->
        <div>
            <div class="menu-select" @click="handleClickType">Type</div>
            <div class="menu-select" type="file" accept="text/*">
                <label for="cus-input-file">Upload</label>
                <input id="cus-input-file" type="file" accept="text/*" v-show="false" />
            </div>
        </div>
    </q-page>
    <q-dialog v-model="uploadDialog" persistent>
        <q-card style="min-width: 350px">
            <q-card-section>
                <div class="text-h6">Your address</div>
            </q-card-section>

            <!-- <q-card-section class="q-pt-none">
                <q-input dense v-model="address" autofocus @keyup.enter="prompt = false" />
            </q-card-section> -->

            <q-card-actions align="right" class="text-primary">
                <q-btn flat label="Cancel" v-close-popup />
                <q-btn flat label="Add address" v-close-popup />
            </q-card-actions>
        </q-card>
    </q-dialog>
</template>

<script>
import { defineComponent } from 'vue';
import { useRouter } from 'vue-router';

export default defineComponent({
    name: 'PageIndex',
    setup() {
        const $router = useRouter();

        const handleClickType = function () {
            $router.push('/type');
        };

        // const

        // const handleClickUpload = function() {

        // }

        const uploadDialog = ref(false);

        return {
            handleClickType,
            uploadDialog,
        };
    },
});
</script>

<style lang="sass" scoped>
.menu-select
    font-size: 20px
    color: white
.menu-select:hover
    color: red
</style>
