<template>
    <div class="fixed-center" style="width: 100%; height: 60px; background: linear-gradient(#333437, 35%, black, 65%, #333437)"></div>
    <div class="row justify-center" ref="contentBlock" tabindex="0" @keydown.self="handleKeydown">
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
import { useRoute } from 'vue-router';

import _ from 'lodash';

import ChunkBlock from 'components/ChunkBlock.vue';

import TextFile from 'assets/keyboard.json';

export default {
    components: {
        ChunkBlock,
    },
    setup() {
        const $store = useStore();
        const $route = useRoute();
        // const $router = useRouter();

        // 타이핑 할 문자 chunk 단위로 분리
        const content = ref(TextFile.text);
        if ($route.params.content !== undefined) {
            content.value = $route.params.content;
        }

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
        const focusingIndex = ref(-1);
        const focusNextLine = function () {
            if (focusingIndex.value >= chunkList.value.length) alert('end');
            else focusingIndex.value += 1;
        };

        const focusingTop = computed({
            get: () => pageSize.value.height / 2 - 40,
        });

        const contentBlock = ref(null);
        const handleKeydown = function (event) {
            if (event.keyCode === 13 || event.keyCode === 9) {
                focusNextLine();
            }
        };

        // 결과값 계산
        const typoCnt = ref(0);
        const letterCnt = ref(0);
        const typingTimer = ref({});

        const checkRenderRange = function (idx) {
            return Math.abs(focusingIndex.value - idx) < pageSize.value.height / (48 * 2) ? true : false;
        };

        onMounted(() => {
            makeParagraphList(content.value);
            paragraphList.value.forEach((paragraph) => {
                makeChunkList(paragraph);
            });
            contentBlock.value.focus();
        });

        return {
            chunkList,

            pageSize,
            checkLeastSize,

            focusingIndex,
            focusNextLine,
            focusingTop,

            contentBlock,
            handleKeydown,

            checkRenderRange,
        };
    },
};
</script>
