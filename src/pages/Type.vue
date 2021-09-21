<template>
    <div>
        <div class="absolute-center bg-cusgrey" style="width: 700px">
            <template v-for="(chunk, idx) in chunkList" :key="idx">
                <ChunkBlock :chunk="chunk"></ChunkBlock>
            </template>
        </div>
    </div>
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
        const content = ref(TextFile.text);
        const paragraphList = ref([]);
        // _.split(textFile.value, '.')
        const chunkList = ref([]);
        // _.chunk(paragraphList.value[0], 50)

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

        onMounted(() => {
            makeParagraphList(content.value);
            paragraphList.value.forEach((paragraph) => {
                makeChunkList(paragraph);
            });
            // makeChunkList('가나다 라마바 사아자');
        });

        return {
            chunkList,
        };
    },
};
</script>
