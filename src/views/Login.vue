<template lang='pug'>
  .login
    transition(name='popover')
      settingsWindow(v-if='openSettings')
    .login-body
      BackgroundImage
      LoginComponent
      settings-button(:class='["settings", (!openLogin) ? "hide" : ""]')
</template>

<script>
import SettingsWindow from '@/components/SettingsWindow.vue';
import LoginComponent from '@/components/LoginComponent.vue';
import BackgroundImage from '@/components/BackgroundImage';
import SettingsButton from '@/components/SettingsButton';
import {mapState, mapMutations} from 'vuex'

export default {
  name: 'login',
  components: {
    BackgroundImage,
    LoginComponent,
    SettingsWindow,
    SettingsButton
  },
  mounted() {
    setTimeout(() => {
      this.SET({type: 'openLogin', items: true});
    }, 400)
  },
  computed: {
    ...mapState(['openSettings', 'openLogin']),
  },
  methods: {
    ...mapMutations(['SET']),
  }
}
</script>

<style lang="stylus">
.settings
  transition .5s

  position absolute
  right 100px
  bottom 70px

  &.hide
    transform translateX(200px)
    
.popover-enter,
.popover-leave-to 
  opacity 0
  transform: translate(-50%, -50%) perspective(900px) rotate3d(70, 0, 0, -30deg);

.popover-enter-to,
.popover-leave 
  opacity 1
  transform: translate(-50%, -50%) perspective(900px) rotate3d(0, 0, 0, 0deg);

.popover-enter-active
  transition .4s

.popover-leave-active 
  transition .2s

.login
  display flex
  height 100vh
  overflow hidden

.login-body
  position relative
  flex 1 0 auto
</style>
