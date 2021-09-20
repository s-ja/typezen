<template>
    <div class="q-my-sm" style="height: 2rem">
        <div>{{ chunk }}</div>
        <div>
            <input type="text" />
        </div>
    </div>
</template>

<script>
export default {
    props: ['chunk'],
};
</script>
