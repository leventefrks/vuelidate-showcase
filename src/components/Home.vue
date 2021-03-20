<template>
  <div class="flex items-center min-h-screen bg-gray-200 dark:bg-gray-900">
    <div class="container mx-auto">
      <div class="max-w-md mx-auto my-10 bg-white p-5 rounded-md shadow-xl">
        <div class="text-center">
          <h1
            class="my-3 text-3xl font-semibold text-gray-700 dark:text-gray-200"
          >
            Vuelidate
          </h1>
          <p class="text-gray-400 dark:text-gray-400">
            Fill up the form below to send us a message.
          </p>
        </div>
        <div class="m-7">
          <form id="form" @submit.prevent="submitForm">
            <div class="mb-6">
              <label
                for="name"
                class="block mb-2 text-sm text-gray-600 dark:text-gray-400"
                >First Name</label
              >
              <input
                type="text"
                name="name"
                id="name"
                placeholder="John Doe"
                v-model="form.name"
                @blur="$v.form.name.$touch()"
                @input="$v.form.name.$touch()"
                class="w-full px-3 py-2 placeholder-gray-300 border border-gray-300 rounded-md focus:outline-none dark:bg-gray-700 dark:text-white dark:placeholder-gray-500 dark:border-gray-600"
                :class="{
                  'border-red-500': shouldAppendErrorClass($v.form.name),
                  'border-green-500': shouldAppendValidClass($v.form.name),
                }"
              />
              <div v-if="$v.form.name.$error" class="w-full mt-2 text-red-500">
                The name field is required
              </div>
            </div>
            <div class="mb-6">
              <label
                for="github-user-name"
                class="block mb-2 text-sm text-gray-600 dark:text-gray-400"
                >Github user name</label
              >
              <input
                type="text"
                name="github-user-name"
                id="github-user-name"
                placeholder="Github username"
                v-model.lazy="$v.form.githubUsername.$model"
                class="w-full px-3 py-2 placeholder-gray-300 border border-gray-300 rounded-md focus:outline-none dark:bg-gray-700 dark:text-white dark:placeholder-gray-500 dark:border-gray-600"
                :class="{
                  'border-red-500': shouldAppendErrorClass(
                    $v.form.githubUsername
                  ),
                  'border-green-500': shouldAppendValidClass(
                    $v.form.githubUsername
                  ),
                }"
              />
              <div v-show="$v.form.githubUsername.$pending" class="w-full mt-2">
                Loading...
              </div>
              <div
                v-if="
                  !$v.form.githubUsername.$exists &&
                  $v.form.githubUsername.$error
                "
                class="w-full mt-2 text-red-500"
              >
                The Github username doesn't exist
              </div>
            </div>
            <div class="mb-6">
              <label class="inline-flex items-center mt-3 select-none">
                <input
                  type="checkbox"
                  class="form-checkbox h-5 w-5 text-gray-600"
                  v-model="form.newsletter"
                /><span class="ml-2 text-gray-700"
                  >Subscribe to our newsletter</span
                >
              </label>
            </div>
            <div class="mb-6">
              <label
                for="email"
                class="block mb-2 text-sm text-gray-600 dark:text-gray-400"
                >Age</label
              >
              <input
                type="text"
                name="email"
                id="email"
                placeholder="your age"
                v-model="form.age"
                @blur="$v.form.age.$touch()"
                @input="$v.form.age.$touch()"
                class="w-full px-3 py-2 placeholder-gray-300 border border-gray-300 rounded-md focus:outline-none dark:bg-gray-700 dark:text-white dark:placeholder-gray-500 dark:border-gray-600"
                :class="{
                  'border-red-500': shouldAppendErrorClass($v.form.age),
                  'border-green-500': shouldAppendValidClass($v.form.age),
                }"
              />
              <div
                v-if="!$v.form.age.required && $v.form.age.$error"
                class="w-full mt-2 text-red-500"
              >
                The age field is required
              </div>
              <div
                v-else-if="!$v.form.age.integer && $v.form.age.$error"
                class="w-full mt-2 text-red-500"
              >
                The age field should be a number
              </div>
              <div
                v-else-if="!$v.form.age.between && $v.form.age.$error"
                class="w-full mt-2 text-red-500"
              >
                The age field should be between 12 and 120
              </div>
            </div>
            <div class="mb-6">
              <label
                for="name"
                class="block mb-2 text-sm text-gray-600 dark:text-gray-400"
                >Email</label
              >
              <input
                type="text"
                name="email"
                id="email"
                placeholder="your email"
                v-model="form.email"
                @blur="$v.form.email.$touch()"
                @input="$v.form.email.$touch()"
                class="w-full px-3 py-2 placeholder-gray-300 border border-gray-300 rounded-md focus:outline-none dark:bg-gray-700 dark:text-white dark:placeholder-gray-500 dark:border-gray-600"
                :class="{
                  'border-red-500': shouldAppendErrorClass($v.form.email),
                  'border-green-500': shouldAppendValidClass($v.form.email),
                }"
              />
              <div
                v-if="!$v.form.email.required && $v.form.email.$error"
                class="w-full mt-2 text-red-500"
              >
                The email field is required so we can send our newsletter
              </div>
              <div
                v-else-if="!$v.form.email.$email && $v.form.email.$error"
                class="w-full mt-2 text-red-500"
              >
                The email field is invalid
              </div>
            </div>
            <div class="mb-6">
              <button
                type="submit"
                class="w-full px-3 py-4 text-white bg-indigo-500 rounded-md focus:bg-indigo-600 focus:outline-none"
              >
                Submit
              </button>
            </div>
            <p
              v-if="!$v.form.$invalid"
              class="text-base text-center text-green-500"
            >
              The form is valid
            </p>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {
  required,
  requiredIf,
  integer,
  between,
  email,
  helpers,
} from 'vuelidate/lib/validators';
import axios from 'axios';

export default {
  name: 'Home',

  data() {
    return {
      form: {
        name: null,
        age: null,
        email: null,
        newsletter: null,
        githubUsername: null,
      },
    };
  },

  validations: {
    form: {
      name: {
        required,
      },

      githubUsername: {
        exists(value) {
          if (!helpers.req(value)) {
            return true;
          } else {
            return axios.get(`//api.github.com/users/${value}`);
          }
        },
      },

      age: {
        required,
        integer,
        between: between(12, 120),
      },

      email: {
        email,
        required: requiredIf(function () {
          return !!this.form.newsletter;
        }),
      },

      newsletter: {
        required,
      },
    },
  },

  computed: {},

  methods: {
    shouldAppendValidClass(field) {
      return !field.$invalid && field.$model && field.$dirty;
    },

    shouldAppendErrorClass(field) {
      return field.error;
    },

    submitForm() {
      this.$v.form.$touch();
      if (!this.$v.form.$invalid) {
        console.log('form - submitted');
      } else {
        console.log('invalid form');
      }
    },
  },
};
</script>
