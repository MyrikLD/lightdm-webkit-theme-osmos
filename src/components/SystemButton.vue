<template lang='pug'>
  .system-button(@click='clickSystemButton')
    system-icons(:type='type')
</template>

<script>
  import { mapState, mapGetters, mapMutations } from 'vuex'
  import SystemIcons from '@/components/SystemIcons';

  export default {
    name: 'system-button',
    props: ['type', 'disabled'],
    components: {
      SystemIcons
    },
    computed: {
      ...mapState(['openSettings']),
    },
    methods: {
      ...mapMutations(['SET']),
      clickSystemButton() {
        if (this.type === 'settings') {
          this.SET({type: 'openLogin', items: false})
          this.SET({type: 'openSettings', items: true})
        } else {
          setTimeout(lightdm[this.type], 500);
        }
      }
    },
  }
</script>

<style lang='stylus'>
.system-button
  max-width 50px
  max-height 50px
  overflow hidden
  svg
    margin-top 20%
    width 60%
    height 60%
</style>
