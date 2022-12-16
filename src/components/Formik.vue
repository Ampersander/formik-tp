<script setup>

    import { defineProps, provide, reactive, ref } from 'vue'

    const props = defineProps({
        onSubmit: {
            type: Function,
            required: true
        },
        initialValues: {
            type: Object,
            required: true
        },
        validate: {
            type: Function,
            required: true
        }
    })

    // Create a reactive state
    const state = reactive({
        values: props.initialValues,
        errors: {},
    })

    const setValues = (name, value) => {
        state.values = {
            ...state.values,
            [name]: value
        };
    }

    const setErrors = (errors) => {
        state.errors = errors;
        isSubmitting.value = false;
    }

    const isSubmitting = ref(false);

    const formSubmit = () => {
        const errors = props.validate(state.values);
        console.log(state)
        // If there are no errors, submit the form
        if (errors.length === 0) {
            isSubmitting.value = true;
            props.onSubmit(state.values);
        } else {
            //Change the state of the errors
            setErrors(errors);
        }
    }

    // Pass the values and errors to the child components
    provide('formik', {
        values: state.values,
        errors: state.errors,
        setValues
    })
</script>

<template>
    <slot 
        :values="values" 
        :errors="errors" 
        @update:value="val" 
        :handleSubmit="formSubmit" 
    />
</template>