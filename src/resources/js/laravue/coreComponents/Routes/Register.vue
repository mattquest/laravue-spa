<template>
  <v-row class="mt-sm-5 pt-sm-5">
    <v-flex>
      <div class="pa-3 display-1 secondary--text text-center">
        Register
      </div>
      <v-card class="elevation-3 mx-auto" max-width="450">
        <v-card-text>
          <v-form>
            <v-text-field
              v-model="form.fields.name"
              prepend-icon="fa-user"
              name="name"
              label="Name"
              :error-messages="form.errors.name"
              @keydown="form.resetStatus()"
              type="text"
            ></v-text-field>
            <v-text-field
              v-model="form.fields.email"
              prepend-icon="fa-envelope"
              name="email"
              label="Email"
              :error-messages="form.errors.email"
              @keydown="form.resetStatus()"
              type="text"
            ></v-text-field>
            <v-text-field
              v-model="form.fields.password"
              id="password"
              label="Password"
              name="password"
              prepend-icon="fa-lock"
              type="password"
              :error-messages="form.errors.password"
              @keydown="form.resetStatus()"
              @keydown.enter="login()"
            ></v-text-field>
            <v-text-field
              v-model="form.fields.password_confirmation"
              id="password_confirmation"
              label="Confirm Password"
              name="password_confirmation"
              prepend-icon="fa-lock"
              type="password"
              @keydown="form.resetStatus()"
              @keydown.enter="login()"
            ></v-text-field>
            <v-layout justify-end row>
              <v-spacer></v-spacer>
              <v-flex text-xs-right>
                <v-checkbox
                  v-model="form.fields.terms"
                  :error-messages="form.errors.terms"
                >
                  <template slot="label">
                    <v-flex>
                      Agree to
                      <a target="_blank" href="/terms" @click.stop>
                        Terms of Use
                      </a>
                    </v-flex>
                  </template>
                </v-checkbox>
              </v-flex>
            </v-layout>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text :disabled="pending" @click="form.reset()">reset</v-btn>
          <v-btn
            color="primary"
            :disabled="pending"
            :loading="pending"
            @click="register()"
            >Register</v-btn
          >
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-row>
</template>

<script>
  import Form from "@wheelmaker/js-form"
  export default {
    name: "Register",
    data() {
      return {
        pending: false,
        form: new Form(
          "/register",
          {
            name: "",
            email: "",
            password: "",
            password_confirmation: "",
            terms: false
          },
          r => r.response.data.errors
        )
      }
    },
    menuItem: true,
    guest: true,
    route: {
      name: "register",
      path: "register"
    },
    methods: {
      register() {
        this.pending = true
        this.form
          .post()
          .then(() => {
            location.reload()
          })
          .catch(r => {
            this.pending = false
          })
      }
    }
  }
</script>

<style scoped></style>
