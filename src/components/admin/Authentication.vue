<template>
  <div class="my-2">
    <h4 class="bg-primary text-white text-center p-2">
      Panel administracyjny sklepu sportowego
    </h4>
    <h4 v-if="showFailureMessage" class="bg-danger text-white text-center p-2 my-2">
      Uwierzytelnianie nie powiodło się. Spróbuj ponownie.
    </h4>
    <div class="form-group">
      <label>Nazwa użytkownika</label>
      <input class="form-control" v-model="$v.username.$model">
      <validation-error v-bind:validation="$v.username" />
    </div>
    <div class="form-group">
      <label>Hasło</label>
      <input type="password" class="form-control" v-model="$v.password.$model">
      <validation-error v-bind:validation="$v.password" />
    </div>
    <div class="text-center">
      <button class="btn btn-primary" v-on:click="handleAuth">Zaloguj</button>
    </div>
  </div>
</template>

<script>
  import { required } from "vuelidate/lib/validators";
  import { mapActions, mapState } from "vuex";
  import ValidationError from "../ValidationError";
  export default {
    name: "Authentication",
    components: {ValidationError},
    data () {
      return {
        username: null,
        password: null,
        showFailureMessage: false,
      }
    },
    computed: {
      ...mapState({
        authenticated: state => state.auth.authenticated
      })
    },
    validations: {
      username: { required },
      password: { required }
    },
    methods: {
      ...mapActions(["authenticate"]),
      async handleAuth() {
        this.$v.$touch();
        if (!this.$v.$invalid) {
          await this.authenticate({name: this.username, password: this.password});
          if (this.authenticated) {
            this.$router.push("/admin");
          } else {
            this.showFailureMessage = true;
          }
        }
      }
    }
  }
</script>

<style scoped>

</style>>