<template>
  <q-page class="flex flex-center">
    <div>
      <div class="layout-padding">
        <div v-if="!authenticated">
          <div class="column items-center">
            <div class="auto"><img src="~assets/quasar-logo.png"></div>
            <div class="auto">
              <h3>
                </br>
                <router-link to="/signin">Войти</router-link> или <router-link to="/register">зарегистрироваться</router-link> в чат Кристины
                </br></br>
              </h3>
            </div>
            <div class="auto"><img src="~assets/feathers-logo.png"></div>
          </div>
        </div>
        <div v-else class="column items-center">
          <h5>
            Перейти в <router-link to="/chat">Чат</router-link>
          </h5>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
  export default {
    props: ['user'],
    data () {
      return {
      }
    },
    computed: {
      authenticated () {
        return this.user !== null
      }
    },
    methods: {
    },
    mounted () {
    },
    beforeDestroy () {
    }
  }
</script>

<style lang="styl">

</style>
