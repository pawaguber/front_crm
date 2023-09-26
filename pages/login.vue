<template>
  <v-form v-model="valid" @submit.prevent="onSubmit">
    <v-container>
      <v-row>
        <v-col cols="12" md="4">
          <v-text-field
              v-model="email"
              :rules="emailRules"
              label="E-mail"
              hide-details
              required
          ></v-text-field>
        </v-col>

        <v-col cols="12" md="4">
          <v-text-field
              v-model="password"
              :rules="passwordRules"
              label="Password"
              :type="isPasswordVisible ? 'text' : 'password'"
              :append-inner-icon="isPasswordVisible ? 'mdi-eye-off-outline' : 'mdi-eye-outline'"
              @click:append-inner="isPasswordVisible = !isPasswordVisible"
              hide-details
              required
          ></v-text-field>
        </v-col>
      </v-row>
    </v-container>

    <VBtn
        block
        type="submit"
    >
      Login
    </VBtn>

    <v-alert
        v-if="error"
        color="error"
        icon="$error"
        title="Сталася помилка"
        :text="error.value"
    ></v-alert>
  </v-form>
</template>

<script setup>

import axios from "axios";
const route = useRoute()
const router = useRouter()

const email = ref('');
const password = ref('');
const isPasswordVisible = ref(false);
const valid = ref(false);
const error = ref('')

const onSubmit = () => {
  if (valid) {
    axios.post('http://127.0.0.1:8000/api/auth/login', {
      email: email.value,
      password: password.value
    }).then(r => {
      console.log(r)
      const {user} = r.data.data;
      const {token} = r.data.data;

      localStorage.setItem('user', JSON.stringify(user))
      localStorage.setItem('accessToken', token)

      router.replace(route.query.to ? String(route.query.to) : '/dashboard/')
    }).catch(err => {
      console.log(err)
      error.value = 'Щось сталось не так'
    })
  }
}


const emailRules = [
  value => !!value || 'Please type email',
  value => /.+@.+/.test(value) || 'Invalid Email address',
];

const passwordRules = [
  value => !!value || 'Please type password',
  value => (value && value.length >= 8) || 'Min 8',
];
</script>
