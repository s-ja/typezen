<template>
    <div class="q-my-sm" style="height: 3rem">
        <div>
            <template v-for="(ch, idx) in chunk" :key="idx">
                <span :style="{ color: typoCheckList[idx] ? '#ff0000' : '#c6c6c6' }">{{ ch }}</span>
            </template>
        </div>
        <div>
            <input class="q-pa-none" v-model="usrInput" :maxlength="chunk.length" style="width: 100%; border: none; color: white; background-color: #333437" />
        </div>
    </div>
</template>

<script>
import { ref, computed } from 'vue';

export default {
    props: ['chunk'],
    setup(props) {
        const usrInput = ref('');

        const currentIdx = computed({
            get: () => usrInput.value.length - 1,
        });

        const typoCheckList = computed({
            get: () => {
                let list = [];
                for (let i = 0; i < usrInput.value.length; i++) {
                    list.push(usrInput.value[i] !== props.chunk[i]);
                }
                return list;
            },
        });

        return {
            usrInput,
            typoCheckList,
        };
    },
};
</script>
<style lang="sass" scoped>
input:focus
    outline: none
</style>
