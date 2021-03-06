<template>
  <div class="container d-flex">
    <div class="search-form align-items-center">
      <div class="text-center mb-4">
        <img class="mb-4" src="../assets/logo1.svg" alt width="150px" />
        <h1 class="h3 mb-3 font-weight-normal">Find Dev</h1>
        <span>
          Digite o nome de usuário do
          <a href="https://www.github.com" target="_blank">GitHub</a> de um desenvolvedor para
          visualizar o perfil e seus repositórios.
        </span>
      </div>
      <div class="form-label-group">
        <input
          v-model="username"
          v-on:keyup.enter="fetchUser(username)"
          v-on:keyup="not_found = false"
          v-bind:class="{ 'is-invalid': not_found }"
          type="text"
          id="username"
          class="form-control"
          placeholder="Username"
          required
          autofocus
        />
        <div class="invalid-feedback text-center" v-if="not_found">
          Não encontramos o Dev que você procurava. Sorry!
        </div>
        <label for="username">Username</label>
      </div>
      <button
        class="btn btn-lg btn-primary btn-block"
        v-on:click="fetchUser(username)"
        :disabled="loading || !isValid"
      >
        <i class="fa fa-search" v-if="!loading"></i> <span v-else>Loading...</span>
      </button>
    </div>
  </div>
</template>

<script>
import { getUser } from '@/services/users';

export default {
  name: 'Find',
  data() {
    return {
      username: '',
      loading: false,
      not_found: false,
    };
  },
  methods: {
    fetchUser(username) {
      this.loading = true;
      getUser(username)
        .then((response) => response.data)
        .then((user) => {
          this.$router.push({
            path: `/find/${user.login}`,
          });
        })
        .then(() => {
          this.loading = false;
        })
        .catch(() => {
          this.not_found = true;
          this.loading = false;
        });
    },
  },
  computed: {
    isValid() {
      return this.username !== '';
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  height: 100%;
}
.search-form {
  width: 100%;
  max-width: 420px;
  padding: 15px;
  margin: auto;
}

.form-label-group {
  position: relative;
  margin-bottom: 1rem;
}

.form-control:focus {
  color: #495057;
  background-color: #fff;
  border-color: #484d52;
  outline: 0;
  -webkit-box-shadow: 0 0 0 0.2rem rgba(21, 22, 22, 0.25);
  box-shadow: 0 0 0 0.2rem rgba(21, 22, 22, 0.25);
}

.form-label-group > input,
.form-label-group > label {
  height: 3.125rem;
  padding: 0.75rem 0.75rem 0.75rem 1.2rem;
}

.form-label-group > input {
  border-radius: 25px;
}

.form-label-group > label {
  position: absolute;
  top: 0;
  left: 0;
  display: block;
  width: 100%;
  margin-bottom: 0; /* Override default `<label>` margin */
  line-height: 1.5;
  color: #495057;
  pointer-events: none;
  cursor: text; /* Match the input under the label */
  border: 1px solid transparent;
  border-radius: 0.25rem;
  transition: all 0.1s ease-in-out;
}

.form-label-group input::placeholder {
  color: transparent;
}

.form-label-group input:not(:placeholder-shown) {
  padding-top: 1.25rem;
  padding-bottom: 0.25rem;
}

.form-label-group input:not(:placeholder-shown) ~ label {
  padding-top: 0.25rem;
  padding-bottom: 0.25rem;
  font-size: 12px;
  color: #777;
}

.btn-lg {
  border-radius: 25px;
}
</style>
