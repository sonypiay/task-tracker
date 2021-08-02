<template>
    <button 
    @click="onClick()" 
    @toggle-add-task="$emit('toggle-add-task')"
    class="btn">{{ text }}</button>
</template>

<script>
export default {
    name: 'Button',
    props: {
        text: String
    },
    methods: {
        onClick() {
            this.$emit('toggle-add-task');
        }
    }
}
</script>