<template>
  <v-app id="inspire">
    <v-content>
      <v-container
        class="fill-height"
        fluid
      >
        <v-row
          align="center"
          justify="center"
        >
          <v-col
            cols="12"
            sm="8"
            md="4"
          >
            <v-card class="elevation-12">
              <v-toolbar
                color="primary"
                dark
                flat
              >
                <v-toolbar-title>Login</v-toolbar-title>
                <v-spacer />
                 <v-progress-circular
                 v-if="status=== 'loading' "
                  :size="50"
                  color="white"
                  indeterminate
                ></v-progress-circular>
              </v-toolbar>
              <v-card-text>
                <v-form>
                  <v-text-field
                prepend-icon="email"
                name="email"
                label="Email"
                type="email"
                :error-messages="emailErrors"
                :success="!$v.user.email.$invalid"
                v-model.trim="$v.user.email.$model"
              ></v-text-field>
              <v-text-field
                prepend-icon="lock"
                name="password"
                label="Senha"
                type="password"
                 :success="!$v.user.password.$invalid"
                :error-messages="passwordErrors"
                v-model.trim="$v.user.password.$model"
              ></v-text-field>
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-spacer />
                <v-btn 
                :disabled="$v.$invalid"
                color="secondary"
                @click="submit"
                >Login</v-btn>
                <v-btn color="secondary"
                @click="$router.push('/cadastro')"
                >Cadastro</v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import {createNamespacedHelpers} from 'vuex'
import { required, email, minLength } from 'vuelidate/lib/validators'

const { mapActions, mapState } = createNamespacedHelpers('login')

export default {
  name: 'Login',
  data: () => ({
    user: {
      email: '',
      password: ''
    }
  }),
    validations:  {
      user: {
        email: {
          required,
          email
        },
        password: {
          required,
          minLength: minLength(6)
        }
    }
  },
  computed: {
    ...mapState(["erroLogin","status"]),
    emailErrors () {
      const errors = []
      const email = this.$v.user.email
      if (!email.$dirty) { return errors }
      !email.required && errors.push('Email é obrigatório!')
      !email.email && errors.push('Insira um email válido!')
      return errors
    },
    passwordErrors () {
      const errors = []
      const password = this.$v.user.password
      if (!password.$dirty) { return errors }
      !password.required && errors.push('Senha é obrigatória!')
      !password.minLength && errors.push(`Insira pelo menos ${password.$params.minLength.min} caracteres!`)
      return errors
    },
     
  },
   methods: {
     ...mapActions(['logar']),
		 async	submit () {
         await	 this.logar({
							 email: this.user.email,
							 senha: this.user.password
           })
           console.log('dsadasdasudasgdhkask')
           if(this.erroLogin == null) this.$router.push('/home')
           else
           console.log(this.erroLogin)
         }
    },
    created(){
      console.log("Status", this.status)
    }
}

</script>
