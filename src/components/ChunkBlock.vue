<template>
    <div style="height: 48px" :style="{ opacity: isFocused ? 1 : 0.5, pointerEvents: isFocused ? 'auto' : 'none' }">
        <div class="q-my-sm">
            <div>
                <template v-for="(ch, idx) in chunk" :key="idx">
                    <span :style="{ color: typoCheckList[idx] ? '#ff0000' : '#c6c6c6' }">{{ ch }}</span>
                </template>
            </div>
            <div>
                <input ref="textbox" class="q-pa-none" v-model="usrInput" :maxlength="chunk.length" @keydown="handleKeydown" style="width: 100%; border: none; color: white; background: none" />
            </div>
        </div>
    </div>
</template>

<script>
import { ref, computed, watch } from 'vue';

export default {
    props: ['chunk', 'isFocused'],
    emit: ['inputEnd'],
    setup(props, { emit }) {
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

        const handleKeydown = function (event) {
            if (event.keyCode === 13 || event.keyCode === 9) {
                emit('inputEnd');
                event.preventDefault();
            }
        };

        const textbox = ref(null);
        const typoCnt = ref(0);
        const countWrong = function () {
            typoCnt.value = props.chunk.length - usrInput.value.length;
            for (let i = 0; i < usrInput.value.length; i++) {
                if (props.chunk[i] !== usrInput.value[i]) typoCnt.value += 1;
            }
            console.log(typoCnt.value);
        };
        watch(
            () => props.isFocused,
            (isFocused) => {
                if (isFocused) textbox.value.focus();
                if (!isFocused) {
                    countWrong();
                }
            }
        );

        return {
            usrInput,
            typoCheckList,
            handleKeydown,
            textbox,
        };
    },
};
</script>
<style lang="sass" scoped>
input:focus
    outline: none
</style>
