<template>
    <div class="fixed-center" style="width: 100%; height: 60px; background: linear-gradient(#333437, 35%, black, 65%, #333437)"></div>
    <div class="row justify-center" style="width: 100%; height: 100%">
        <div class="bg-cusgrey" v-if="checkLeastSize">
            <div style="width: 700px">
                <template v-for="(chunk, idx) in chunkList" :key="idx">
                    <div class="absolute" v-if="checkRenderRange(idx)" :style="{ top: focusingTop + (idx - focusingIndex) * 48 + 'px' }">
                        <ChunkBlock :chunk="chunk" :isFocused="idx === focusingIndex" @inputEnd="focusNextLine"></ChunkBlock>
                    </div>
                </template>
            </div>
        </div>
        <div v-else>화면이 너무 작습니다.</div>
    </div>
</template>
<script>
import { ref, computed, onMounted } from 'vue';
import { useStore } from 'vuex';

import _ from 'lodash';

import ChunkBlock from 'components/ChunkBlock.vue';

import TextFile from 'assets/keyboard.json';

export default {
    components: {
        ChunkBlock,
    },
    setup() {
        const $store = useStore();

        // 타이핑 할 문자 chunk 단위로 분리
        const content = ref(TextFile.text);
        const paragraphList = ref([]);
        const chunkList = ref([]);

        const makeParagraphList = function (content) {
            paragraphList.value = _.split(content, '\n');
        };

        const makeChunkList = function (paragraph) {
            let wordList = _.split(paragraph, ' ');
            let chunk = '';
            wordList.forEach((word) => {
                if (chunk.length + word.length > 52) {
                    chunkList.value.push(chunk.substr(1));
                    chunk = '';
                }
                chunk = chunk.concat(' '.concat(word));
            });
            chunkList.value.push(chunk.substr(1));
        };

        // page resize fit
        const pageSize = computed(() => $store.getters['comm/pageSize']);
        const checkLeastSize = computed({
            get: () => {
                return pageSize.value.width > 740 && pageSize.value.height > 200;
            },
        });

        // focus 된 chunk
        const focusingIndex = ref(0);
        const focusingTop = computed({
            get: () => pageSize.value.height / 2 - 40,
        });
        // const chunkBlockTop = computed({
        //     get: (idx) => ,
        // });
        const focusNextLine = function () {
            console.log('receive');
            focusingIndex.value += 1;
        };

        const checkRenderRange = function (idx) {
            return Math.abs(focusingIndex.value - idx) < 8 ? true : false;
        };

        onMounted(() => {
            makeParagraphList(content.value);
            paragraphList.value.forEach((paragraph) => {
                makeChunkList(paragraph);
            });
        });

        return {
            chunkList,

            checkLeastSize,

            focusingIndex,
            focusingTop,
            focusNextLine,

            checkRenderRange,
        };
    },
};
</script>
