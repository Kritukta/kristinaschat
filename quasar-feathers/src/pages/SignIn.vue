<template>
  <q-page class="flex flex-center">
    <q-dialog v-model="showDialog" :title="title" @ok="onOk" @hide="onHide" >
      <div slot="body">
        <div class="row q-mb-md">
          <q-input
            v-model="email" type="email" name="email" stack-label="Логин(почта)" class="full-width" autofocus
          />
        </div>
        <div class="row">
          <q-input
            v-model="password" type="password" name="email" stack-label="Пароль" class="full-width"
          />
        </div>
      </div>
    </q-dialog>
  </q-page>
</template>

<script>
import auth from 'src/auth'

export default {
  data () {
    return {
      showDialog: true,
      email: null,
      password: null,
      title: null
    }
  },
  computed: {
  },
  methods: {
    goHome() {
      this.$router.push({ name: 'home' })
    },
    onHide() {
      // Workaround needed because of timing issues (sequencing of 'hide' and 'ok' events) ...
      setTimeout(() => {
        this.goHome()
      }, 50)

    },
    onOk(data) {
      if (this.isRegistration()) {
        this.register(this.email, this.password)
          .then(() => {
            return this.login(this.email, this.password)
          })
          .then(_ => {
            this.$q.notify({type: 'positive', message: 'Вы успешно зарегистрировались'})
          })
          .catch(_ => {
            this.$q.notify({type: 'positive', message: 'Невозможно зарегистрироваться, пожалуйста проверьте логин или пароль'})
            this.goHome()
          })
      } else {
        this.login(this.email, this.password)
          .then(_ => {
            this.$q.notify({type: 'positive', message: 'Вы успешно залогинились'})
          })
          .catch(_ => {
            this.$q.notify({type: 'positive', message: 'Невозможно залогиниться, пожалуйста проверьте логин или пароль'})
            this.goHome()
          })
      }
    },
    isRegistration () {
      return this.$route.name === 'register'
    },
    register (email, password) {
      return auth.register(email, password)
    },
    login (email, password) {
      return auth.login (email, password)
    }
  },
  mounted () {
    this.title = this.isRegistration() ? 'Регистрация' : 'Вход'
  },
  beforeDestroy () {
  }
}
</script>

<style lang="styl">

</style>
