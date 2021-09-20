<template>
    <div>
        <div class="absolute-center">
            <template v-for="(chunk, idx) in chunkList" :key="idx">
                <ChunkBlock :chunk="chunk"></ChunkBlock>
            </template>
        </div>
    </div>
</template>
<script>
import { ref } from 'vue';

import _ from 'lodash';

import ChunkBlock from 'components/ChunkBlock.vue';

import TextFile from 'assets/keyboard.json';

export default {
    components: {
        ChunkBlock,
    },
    setup() {
        const textFile = ref(TextFile.text);
        const paragraphList = ref(_.split(textFile.value, '.'));
        const chunkList = ref(_.chunk(paragraphList.value[0], 10));

        return {
            chunkList,
        };
    },
};
</script>
