<template>
  <q-layout>
    <q-layout-header>
      <q-toolbar
        color="primary"
        :glossy="false"
        :inverted="$q.theme === 'ios'"
      >
        <q-btn
          flat
          dense
          round
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
          v-show="authenticated"
        >
          <q-icon name="menu" />
        </q-btn>

        <q-toolbar-title>
          Чат Кристины
        </q-toolbar-title>

        <q-btn flat @click="goTo('signin')" v-show="!authenticated">
          Войти
        </q-btn>
        <q-btn flat @click="goTo('register')" v-show="!authenticated">
          Регистрация
        </q-btn>
        <q-btn flat round @click="goTo('home')" v-show="authenticated">
          <q-icon name="home" />
          <q-tooltip anchor="bottom middle" self="top middle" :offset="[0, 20]">Главная</q-tooltip>
        </q-btn>
        <q-btn flat round @click="goTo('chat')" v-show="authenticated">
          <q-icon name="chat" />
          <q-tooltip anchor="bottom middle" self="top middle" :offset="[0, 20]">Чат</q-tooltip>
        </q-btn>
        <q-btn flat round @click="signout" v-show="authenticated">
          <q-icon name="exit_to_app" />
          <q-tooltip anchor="bottom middle" self="top middle" :offset="[0, 20]">Выйти</q-tooltip>
        </q-btn>

      </q-toolbar>
    </q-layout-header>

    <q-layout-drawer
      v-model="leftDrawerOpen"
      v-show="authenticated"
      no-hide-on-route-change
      :content-class="$q.theme === 'mat' ? 'bg-grey-2' : null">

      <q-list
        no-border
        link
        inset-delimiter
      >

        <q-item to="/home">
          <q-item-side icon="home" />
          <q-item-main label="Главная" />
        </q-item>

        <q-item to="/chat">
          <q-item-side icon="chat" />
          <q-item-main label="Чат" />
        </q-item>

        <q-collapsible icon="info" label="О чате">
          <p style="padding: 25px;" class="text-grey-7">
            Это чат Кристины.
          </p>
        </q-collapsible>

      </q-list>
    </q-layout-drawer>

    <q-page-container>
      <router-view :user="user"></router-view>
    </q-page-container>
  </q-layout>
</template>

<script>
import auth from 'src/auth'

export default {
  name: 'index',
  components: {
  },
  data () {
    return {
      leftDrawerOpen: this.$q.platform.is.desktop,
      user: null
    }
  },
  computed: {
    authenticated () {
      return this.$data.user !== null
    }
  },
  methods: {
    goTo (route) {
      this.$router.push({ name: route })
    },
    signout () {
      auth.signout()
        .then(() => {
          this.$q.notify({type: 'positive', message: 'Вы вышли, войдите заново для продолжения работы'})
        })
        .catch(() => {
            this.$q.notify({type: 'positive', message: 'Невозможно выйти, пожалуйста попробуйте заново через несколько минут'})
        })
    },
    setUser (user) {
      this.$data.user = user
    }
  },
  mounted () {
    // Check if there is already a session running
    auth.authenticate()
    .then((user) => {
      this.setUser(user)
      this.$q.notify({type: 'positive', message: 'Восстановление предыдущей сессии'})
    })
    .catch(_ => {
      this.setUser(null)
      this.$router.push({ name: 'home' })
    })

    // On successful login
    auth.onAuthenticated((user) => {
      this.setUser(user)
      this.$router.push({ name: 'home' })
    })

    // On logout
    auth.onLogout(() => {
      this.setUser(null)
      this.$router.push({ name: 'home' })
    })
  },
  beforeDestroy () {
  }
}
</script>

<style>
</style>
