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

        return {
            handleClickType,
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
