<template>
  <div>
    <div v-if="!authenticationState.ready"></div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import { ApplicationPaths, QueryParameterNames } from './ApiAuthorizationConstants';
import authService from './AuthorizeService'
export default Vue.extend({
  data() {
    return {
      subscription: 0,
      authenticationState: {
        ready: false,
        authenticated: false,
      },
    };
  },
  methods: {
    async authenticationChanged() {
      this.authenticationState = {
        ready: false,
        authenticated: false,
      };
      await this.populateAuthenticationState();
    },
    async populateAuthenticationState() {
      const authenticated = await authService.isAuthenticated();
      this.authenticationState = {
        ready: true,
        authenticated: authenticated,
      };

      if (!this.authenticationState.authenticated) {
        window.location.replace(ApplicationPaths.ApiAuthorizationClientConfigurationUrl)
      }
    }
  },
  mounted() {
    this.subscription = authService.subscribe(() => this.authenticationChanged());
    this.populateAuthenticationState();
  },
  destroyed() {
    authService.unsubscribe(this.subscription);
  }
});
</script>
