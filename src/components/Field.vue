<template>
    <component 
        :is="as" 
        :value="formik.values[name]" 
        @input="formik.setValues(name, $event.target.value)"
        @update:modelValue="value => formik.setValues(name, value)"
    />
</template>

<script setup>
    import { inject, defineEmits, defineProps } from 'vue'

    const formik = inject('formik')
    const emit = defineEmits(['update:value'])

    // Get the props from the parent component
    const props = defineProps({
        name: {
            type: String,
            required: true
        },
        as: {
            type: String || Object,
            default: 'input'
        },
    })
</script>