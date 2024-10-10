<script setup lang="ts">
import InputText from '@/components/InputText.vue'
import * as yup from 'yup'
import { useField, useForm } from 'vee-validate'
import { useAuthStore } from '@/stores/auth';
const validationSchema = yup.object({
  firstname: yup.string().required('First name is required'),
  lastname: yup.string().required('Last name is required'),
  email: yup.string().email().required('The email is required'),
  password: yup.string().required('The password is required')
})

const { errors, handleSubmit } = useForm({
  validationSchema,
  initialValues: {
    firstname: '',
    lastname: '',
    email: '',
    password: ''
  }
})
const authStore = useAuthStore()

const { value: firstname } = useField<string>('firstname')
const { value: lastname } = useField<string>('lastname')
const { value: email } = useField<string>('email')
const { value: password } = useField<string>('password')
import { useRouter } from 'vue-router'
const router = useRouter()

import { useMessageStore } from '@/stores/message';
const onSubmit = handleSubmit((values) => {
  const messageStore = useMessageStore()
  authStore.register(values.firstname, values.lastname, values.email, values.password)
  .then(() => {
    router.push({ name: 'login' })  // Redirect to login after registration
  }).catch(() => {
    messageStore.updateMessage('Could not register')
    setTimeout(() => {
      messageStore.resetMessage()
    }, 3000)
  })
})
</script>

<template>
  <div class="flex min h-full flex-1 flex-col justify-center px-6 py-12 lg:px-8">
    <div class="sm:mx-auto sm:w-full sm:max-w-sm">
      <img
        class="mx-auto h-10 w-auto"
        src="https://tailwindui.com/img/logos/mark.svg?color=indigo&shade=600"
        alt="Your Company"
      />
      <h2 class="mt-10 text-center text-2xl font-bold leading-9 tracking-tight text-gray-900">
        Register a new account
      </h2>
    </div>
    <div class="mt-10 sm:mx-auto sm:w-full sm:max-w-sm">
      <form class="space-y-6" @submit.prevent="onSubmit" method="POST">
        <div>
          <label for="firstname" class="block text-sm font-medium leading-6 text-gray-900">First Name</label>
          <InputText
            type="text"
            v-model="firstname"
            placeholder="First Name"
            :error="errors['firstname']"
          />
        </div>
        <div>
          <label for="lastname" class="block text-sm font-medium leading-6 text-gray-900">Last Name</label>
          <InputText
            type="text"
            v-model="lastname"
            placeholder="Last Name"
            :error="errors['lastname']"
          />
        </div>
        <div>
          <label for="email" class="block text-sm font-medium leading-6 text-gray-900">Email Address</label>
          <InputText
            type="email"
            v-model="email"
            placeholder="Email Address"
            :error="errors['email']"
          />
        </div>
        <div>
          <label for="password" class="block text-sm font-medium leading-6 text-gray-900">Password</label>
          <InputText
            type="password"
            v-model="password"
            placeholder="Password"
            :error="errors['password']"
          />
        </div>
        <div>
          <button
            type="submit"
            class="flex w-full justify-center rounded-md bg-indigo-600 px-3 py-1.5 text-sm font-semibold leading-6 text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
          >
            Register
          </button>
        </div>
      </form>
      <p class="mt-10 text-center text-sm text-gray-500">
        Already have an account? {{ '' }}
        <a href="/login" class="font-semibold leading-6 text-indigo-600 hover:text-indigo-500">
          Sign in here
        </a>
      </p>
    </div>
  </div>
</template>
