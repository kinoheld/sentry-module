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

    try {
      this.$sentry.captureEvent({ message: 'This should fail' })
      console.error('The call to captureEvent should fail so this line shouldn\'t be printed (on initial load only, this line will be printed on eg HMR)')
    } catch (err) {
      console.info('Caught expected error on $sentry.captureEvent')
    }

    this.$nextTick(() => {
      console.info('Loading Sentry in 1 second')
      setTimeout(this.$sentryLoad, 1000)
    })
  }
}
</script>
