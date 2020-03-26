<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
  import Stomp from 'stompjs'

  export default {
    name: 'electron-app',
    data () {
      return {
      }
    },
    mounted () {
      this.initStomp()
    },
    methods: {
      initStomp () {
        this.stompClient = Stomp.client('ws://localhost:61614/stomp')
        this.stompClient.heartbeat.outgoing = 2000 // 前端对后台连接心跳监控间隔
        this.stompClient.heartbeat.incoming = 2000 // 后台对前端心跳监控，若为0则不进行心跳监控
        this.stompClient.connect({}, this.connected, this.disconnected)
      },
      connected (f) {
        console.log('connected', f)
        this.stompClient.subscribe('/test', this.onMessage)
        this.send()
      },
      disconnected (e) {
        console.log('disconnected', e)
      },
      send () {
        this.stompClient.send('/test', {}, 'aa')
      },
      onMessage (message) {
        console.log('111', message)
        console.warn(message.body)
      }
    }
  }
</script>

<style>
  /* CSS */
</style>
