<template>
  <div>
    <h3>Server-side</h3>
    <span id="server-side">{{ serverSentry ? 'Works!' : '$sentry object is missing!' }}</span>
    <h3>Client-side</h3>
    <span id="client-side">Works {{ isSentryReady ? 'and is' : 'but is not' }} ready!</span>
  </div>
</template>

<script>
export default {
  data () {
    return {
      isSentryReady: false,
      /** @type {import('@sentry/minimal') | null} */
      serverSentry: this.$sentry
    }
  },
  created () {
    this.$sentryReady().then(() => {
      this.isSentryReady = true
      console.info('Sentry is ready')
    })
  },
  mounted () {
    this.$sentry.captureMessage('Hi!')
  }
}
</script>
