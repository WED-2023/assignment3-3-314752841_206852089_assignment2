<template>
  <div class="register-page">
    <h1>Register</h1>
    <form @submit.prevent="register">
      <div class="form-group">
        <input v-model="state.username" type="text" placeholder="Username" class="form-control" />
        <div v-if="v$.username.$error" class="text-danger">
          Username is required (3-8 characters long).
        </div>
      </div>
      <div class="form-group">
        <input v-model="state.password" type="password" placeholder="Password" class="form-control" />
        <div v-if="v$.password.$error" class="text-danger">
          Password is required (5-10 characters long).
        </div>
      </div>
      <div class="form-group">
        <input v-model="state.confirmPassword" type="password" placeholder="Confirm password" class="form-control" />
        <div v-if="v$.confirmPassword.$error" class="text-danger">
          Passwords must match.
        </div>
      </div>
      <div class="form-group">
        <input v-model="state.firstName" type="text" placeholder="First name" class="form-control" />
        <div v-if="v$.firstName.$error" class="text-danger">
          First name is required.
        </div>
      </div>
      <div class="form-group">
        <input v-model="state.lastName" type="text" placeholder="Last name" class="form-control" />
        <div v-if="v$.lastName.$error" class="text-danger">
          Last name is required.
        </div>
      </div>
      <div class="form-group">
        <select v-model="state.country" type="select" oninput="(this.style='color:black')" class="form-control">
          <option value="" disabled selected style="color:grey">Select country</option>
          <option v-for="country in state.countries" :key="country.label" :value="country.label" style="color:black">
            {{ country.label }}
          </option>
        </select>
        <div v-if="v$.country.$error" class="text-danger">
          Country is required.
        </div>
      </div>
      <div class="form-group">
        <input v-model="state.email" type="email" placeholder="Email" class="form-control" />
        <div v-if="v$.email.$error" class="text-danger">
          Email is required.
        </div>
      </div>
      <div class="form-group">
        <input v-model="state.dateOfBirth" type="text" placeholder="Date of birth" onfocus="(this.type='date')" onblur="(this.type='text')"  class="form-control" />
        <div v-if="v$.dateOfBirth.$error" class="text-danger">
          Date of birth is required.
        </div>
      </div>
      <button type="submit" class="btn btn-success mt-3">Register</button>
    </form>
  </div>
</template>

<script>
import { reactive } from 'vue';
import { useVuelidate } from '@vuelidate/core';
import { required, minLength, maxLength} from '@vuelidate/validators';
import axios from 'axios';
import store from '../store';

const server_domain = store.server_domain;

export default {
  name: "RegisterPage",

  setup(_, { expose }) {
    const state = reactive({
      username: '',
      password: '',
      confirmPassword: '',
      firstName: '',
      lastName: '',
      country: '',
      dateOfBirth: '',
      email: '',

      countries: require('../assets/countries.json').map(country => ({
        label: country.name.common
      }))
    });

    const rules = {
      username: { required, minLength: minLength(3), maxLength: maxLength(8)},
      password: { required, minLength: minLength(5), maxLength: maxLength(10) },
      confirmPassword: { required, sameAs: () => state.confirmPassword === state.password },
      firstName: { required },
      lastName: { required },
      country: {required},
      dateOfBirth: { required },
      email: { required }
    };

    const v$ = useVuelidate(rules, state);

    const register = async () => {
      v$.value.$touch();
      if (v$.value.$invalid) {
        console.error('Validation failed');
        return;
      }

      const response = await axios({
        method: "POST",
        url: server_domain + "/register",
        validateStatus: () => true,
        data: {
        username: state.username,
        firstname: state.firstName,
        lastname: state.lastName,
        country: state.country,
        password: state.password,
        password_confirm: state.confirmPassword,
        email: state.email,
        profilePic: 'none',
        dob: state.dateOfBirth,
        }
      })

      if (response.status === 201) {
        alert('Registration successful!');
        // Optionally, redirect to login or home page
        window.location.href = '/login';
        store.username = state.username;
      }
      else {
        alert('Registration failed.\nErrorCode ' + response.status + ":\n" + response.data.message)
      }
    };

    expose({ register });

    return { state, v$, register };
  }
};
</script>

<style scoped>
.register-page {
  max-width: 400px;
  margin: auto;
}

.register-page h1 {
  text-align: center;
}
</style>
