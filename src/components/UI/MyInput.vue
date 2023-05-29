<script>
export default {
    props: {
        modelValue: [String, Number]
    },
    methods: {
        updateInput(event){
            this.$emit('update:modelValue', event.target.value)
        }
    }
}
</script>

<template>
    <input v-bind:value="modelValue" v-on:input="updateInput" class="input" type="text">
</template>

<style scoped>
.input {
    border: 1px solid teal;
    padding: 10px 15px;
    margin-top: 15px;
}
</style>