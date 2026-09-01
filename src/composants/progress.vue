<template>
    <div>
        <progress :value="value + 1" :max="max"/>
    </div>
</template>



<script setup>
defineProps({
    value : Number,
    max : Number
})

</script>


<style scoped>

    div{
        margin-top: 30px;
    }
</style>