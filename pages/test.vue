<template>
    <v-col class="col-md-5">
      <v-card elevation="5">
        <img :src="require('~/assets/images/banner1.jpg') alt="Cabeçalho">
        <v-card-text class="m-0">
          <h2 class="text-center">
            Gestão de Bots
          </h2>
  
          <v-divider class="my-5" />
  
          <v-form @submit="signIn">
            <span>Usuário:</span>
            <v-text-field
              v-model="username"
              solo
              label="Usuário"
              append-icon="fas fa-user"
            />
  
            <span>Senha:</span>
            <v-text-field
              v-model="password"
              solo
              label="Senha"
              type="password"
              append-icon="fas fa-key"
              required
              class="mb-0"
            />
            <div class="d-flex flex-wrap" flat>
              <v-btn
                block
                color="orange accent-4"
                style="width: 90%"
                type="submit"
                :disabled="isDisabled"
                @click="signIn"
              >
                <span class="text-white">Entrar</span>
              </v-btn>
  
              <v-btn
                color="grey"
                block
                class="mt-3"
                style="width: 90%"
                :disabled="true"
              >
                Esqueci a senha
              </v-btn>
            </div>
          </v-form>
        </v-card-text>
      </v-card>
    </v-col>
  </template>
  
  <script>
  export default {
    name: 'Login',
    layout: 'empty',
    data () {
      return {
        username: '',
        password: '',
      }
    },
    computed: {
      isLogged () {
        return this.$store.state.Auth.isLogged
      },
      isDisabled () {
        if (!this.username || !this.password) {
          return true
        }
  
        if (this.username.length < 4 || this.password.length < 6) {
          return true
        }
  
        return false
      }
    },
    async mounted () {
      await this.$middleware.login('Auth/login')
    },
    methods: {
      async signIn (e) {
        e.preventDefault()
  
        // TODO: mudar lógica de login para bot
        // await this.$middleware.login(this.username, this.password, 'Auth/login')
  
        const params = {
          username: this.username,
          password: this.password
        }
  
        try {
          const response = await this.$middleware.callService('SASBA0100', params)
          this.$store.commit('Auth/SET_USER', response[0])
          this.$storage.set('user', { ...response[0], loginDate: new Date() })
  
          if (this.isLogged) {
            this.$router.push('/')
          }
        } catch (error) {
          this.$toast.error('Login ou senha inválidos.')
        }
      }
    }
  }
  </script>
  
  <style scoped>
  img {
    width: 100%;
    max-height: 400px;
  }
  </style>

<!-- login         // useConversationSummary: 0,
-->

<!-- try {
    const response = await this.$middleware.callService('SASBA0100', params)
    this.$store.commit('Auth/SET_USER', response[0])
    this.$storage.set('user', { ...response[0], loginDate: new Date() })

    if (this.isLogged) {
      this.$router.push('/')
    }
  } catch (error) {
    this.$toast.error('Login ou senha inválidos.')
  }
}
}
} -->