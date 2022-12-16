<script setup>
  import Formik from './components/Formik.vue';
  import Field from './components/Field.vue';

  const onSubmit = (values) => {
    console.log(values);
  };

  const onValidate = (values) => {
    const errors = {};

    if (!values.number) {
      errors.number = 'Required';
    } else if (values.number > 5) {
      errors.number = 'The number must be greater than 5';
    }

    if (!values.name) {
      errors.name = 'Required';
    } else if (values.name.length < 10) {
      errors.name = 'Must be 10 characters or more';
    }

    if (!values.email) {
      errors.email = 'Required';
    } else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i.test(values.email)) {
      errors.email = 'Invalid email address';
    }

    return errors;
  };
</script>

<template>
  <Formik 
    :initialValues="{
      number: 15,
      name: 'name',
      email: 'root@hotmail.fr',
    }" 
    :validate="onValidate" 
    :onSubmit="onSubmit" 
    v-slot="{ formik, handleSubmit }">
    <form @submit.prevent="handleSubmit">
      <p>Name</p>
      <Field name="name" />
      <p>Number</p>
      <Field name="number" type="number" />
      <p>Email</p>
      <Field name="email" />
      <button type="submit">Submit</button>
    </form>
  </Formik>
</template>