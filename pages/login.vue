<template>
  <v-container fill-height class="py-12 px-4" style="min-height: 100vh">
    <v-row class="mt-8 mb-12">
      <v-col :cols="12" :lg="4" :md="5">
        <div class="headline font-weight-bold section-title">
          Login ke Dashboard
        </div>
        <v-alert v-if="error" type="error" class="mt-4">
          {{ error }}
        </v-alert>
        <form @submit.prevent="attemptLogin">
          <v-text-field
            v-model="email"
            class="mt-8"
            label="Alamat E-mail"
            outlined
            dense
          />
          <v-text-field
            v-model="password"
            label="Kata sandi"
            type="password"
            outlined
            dense
          />
          <v-btn class="mt-1 text-none" type="submit" block outlined :loading="isLoggingIn">
            Login
          </v-btn>
          <div class="mt-2">
            <nuxt-link to="/recover" style="text-decoration: none">
              Lupa kata sandi?
            </nuxt-link>
          </div>
        </form>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import { Component, Vue, State, Mutation } from 'nuxt-property-decorator';
import { login, AuthenticationResult } from '~/api/user';

@Component
export default class DashboardLogin extends Vue {
  @State loggedIn!: boolean;
  @Mutation('login') mutationLogin;

  isLoggingIn: boolean = false;
  email: String = '';
  password: String = '';

  error: String = '';

  head() {
    return {
      title: 'Login'
    };
  }

  attemptLogin() {
    this.isLoggingIn = true;
    this.error = '';
    login(this.email, this.password)
      .then((authResult: AuthenticationResult) => {
        this.mutationLogin({
          user: authResult.user,
          bearerToken: authResult.bearerToken
        });

        this.$router.push('/dashboard');
      })
      .catch((e) => {
        this.error = e.toString();
      })
      .finally(() => {
        this.isLoggingIn = false;
      });
  }
}
</script>

<style scoped>
  .section-title {
    position: relative;
    padding-bottom: 8px;
  }

  .section-title:after {
    content: "";
    position: absolute;
    width: 100px;
    height: 1px;
    bottom: 0;
    left: 0;
    border-bottom: 4px solid #FF084F;
  }
</style>