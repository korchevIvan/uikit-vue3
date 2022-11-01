<script setup>
import {ref, computed} from "vue"
import useVuelidate from '@vuelidate/core'
import {helpers, minLength, maxLength, numeric, email, sameAs} from '@vuelidate/validators'

import Input from '@/components/Input.vue'
import Button from '@/components/Button.vue'

const nameField = ref('')
const emailField = ref('')
const luckyField = ref('')
const passwordField = ref('')
const confirmPasswordField = ref('')
const frontendField = ref('')

const mustBeFrontend = (value) => value.includes('frontend')

const rules = computed(() => ({
  nameField: {
    minLength: helpers.withMessage(`Минимальная длина: 3 символа`, minLength(3))
  },
  emailField: {
    email: helpers.withMessage('Вы ввели неверный email', email)
  },
  luckyField: {
    maxLength: helpers.withMessage(`Максимальная длина: 2 символа`, maxLength(2)),
    numeric: helpers.withMessage('Вы можете ввести только цифры', numeric)
  },
  confirmPasswordField: {
    sameAsPassword: helpers.withMessage(`Пароли не совпадают`, sameAs(passwordField.value))
  },
  frontendField: {
    frontendField: helpers.withMessage('Строка должна содержать слово frontend', mustBeFrontend)
  }
}))

const v = useVuelidate(rules, {nameField, emailField, luckyField, confirmPasswordField, frontendField})

const submitForm = () => {
  v.value.$touch()

  if(v.value.$error) return
  alert('Form submitted')
}
</script>

<template>
  <h1 class="heading-1">Inputs</h1>
  <form @submit.prevent="submitForm">
    <Input
        name="nameField"
        placeholder="Input your name"
        label="Your name"
        v-model:value="v.nameField.$model"
        :error="v.nameField.$errors"
    />
    <Input
        name="emailField"
        placeholder="Input your email"
        label="Your email"
        v-model:value="v.emailField.$model"
        :error="v.emailField.$errors"
    />
    <Input
        name="luckyField"
        placeholder="Input your lucky number"
        label="Your lucky number"
        v-model:value="v.luckyField.$model"
        :error="v.luckyField.$errors"
    />

    <Input
        name="passwordField"
        placeholder="Please input password"
        label="Your password"
        v-model:value="passwordField"
        type="password"
    />
    <Input
        name="confirmField"
        placeholder="Please confirm password"
        label="Confirm password"
        v-model:value="v.confirmPasswordField.$model"
        :error="v.confirmPasswordField.$errors"
        type="password"
    />

    <Input
        name="frontend"
        placeholder="Input string with frontend"
        label="Frontend string"
        v-model:value="v.frontendField.$model"
        :error="v.frontendField.$errors"
    />

    <Button label="Submit"/>
  </form>
</template>