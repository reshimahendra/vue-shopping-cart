<template>
  <div class="h-screen flex items-center">
    <div class="w-full max-w-md mx-auto py-12 px-6 bg-white shadow rounded-md">
      <h2 class="flex justify-center">
        <router-link
          to="/"
          class="flex items-center text-6xl text-blue leading-none uppercase relative"
        >
          <span>S</span>
        </router-link>
      </h2>
      <p class="my-5 text-base leading-5 text-center text-gray-900">Log in to your account</p>
      <form @submit.prevent="onSubmit">
        <div class="rounded-md shadow-sm">
          <BaseAlert v-if="error" :message="error" class="bg-red-500" />
          <div class="mb-3">
            <label for="email" class="block mb-2 text-sm text-gray-900">Email *</label>
            <input type="email" class="text-field" placeholder="Email address" v-model="email" />
          </div>
          <div class="mb-3">
            <label for="password" class="block mb-2 text-sm text-gray-900">Password *</label>
            <input
              id="password"
              type="password"
              class="text-field"
              placeholder="Password"
              v-model="password"
            />
          </div>
          <div class="mt-5">
            <button type="submit" class="button button-blue w-full rounded-md">Sign In</button>
          </div>
        </div>
      </form>
      <div class="mt-10">
        <p class="text-center">
          Not registered?
          <router-link to="/signup" class="text-blue underline hover:no-underline">Create an account</router-link>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import appwrite from '@/services/appwrite';
import BaseAlert from '@/components/Base/BaseAlert.vue';

export default {
  name: 'Login',
  components: { BaseAlert },
  data() {
    return {
      email: '',
      password: '',
      error: '',
    };
  },
  methods: {
    async onSubmit() {
      try {
        await appwrite.account.createSession(this.email, this.password);
        const response = await appwrite.account.get();
        this.$store.dispatch('auth/authenticated', response);
        this.$router.push({
          name: 'home',
        });
      } catch (err) {
        this.error = err.message;
      }
    },
  },
};
</script>
