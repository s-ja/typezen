<template>
    <div>
        <div class="absolute-center bg-cusgrey" style="width: 700px">
            <template v-for="(chunk, idx) in chunkList" :key="idx">
                <ChunkBlock :chunk="chunk" v-if="checkRenderRange(idx)"></ChunkBlock>
            </template>
        </div>
    </div>
    <div style="height: 50px; background: linear-gradient(#333437, black, #333437)">gradation</div>
</template>
<script>
import { ref, onMounted } from 'vue';

import _ from 'lodash';

import ChunkBlock from 'components/ChunkBlock.vue';

import TextFile from 'assets/keyboard.json';

export default {
    components: {
        ChunkBlock,
    },
    setup() {
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
                if (chunk.length + word.length + 4 > 60) {
                    chunkList.value.push(chunk.substr(1));
                    chunk = '';
                }
                chunk = chunk.concat(' '.concat(word));
            });
            chunkList.value.push(chunk.substr(1));
        };

        // focus 된 chunk
        const focusingIndex = ref(0);

        const checkRenderRange = function (idx) {
            return Math.abs(focusingIndex.value - idx) < 5 ? true : false;
        };

        onMounted(() => {
            makeParagraphList(content.value);
            paragraphList.value.forEach((paragraph) => {
                makeChunkList(paragraph);
            });
        });

        return {
            chunkList,

            focusingIndex,
            checkRenderRange,
        };
    },
};
</script>
