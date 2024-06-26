<template>
  <div id="app">
    <header class="header">
      <nav
        class="inner"
        role="navigation"
      >
        <router-link
          to="/"
          exact
        >
          <img
            class="logo"
            src="../assets/logo-fastify.svg"
            alt="logo"
          >
          <img
            class="logo"
            src="../assets/logo-vite.svg"
            alt="logo"
          >
        </router-link>
        <router-link
          v-for="feed in $global.feeds"
          :key="feed"
          :to="`/${feed}`"
        >
          {{ capitalize(feed) }}
        </router-link>
        <a
          class="github"
          href="https://github.com/galvez/fastify-vite-vue-hackernews"
          target="_blank"
          rel="noopener banner"
        >
          Built with Fastify and Vite
        </a>
      </nav>
    </header>
    <router-view v-slot="{ Component }">
      <Suspense>
        <component :is="Component" />
      </Suspense>
    </router-view>
  </div>
</template>

<script>
import { useHead } from '@vueuse/head'
import { useRoute } from 'vue-router'

export default {
  setup () {
    const route = useRoute()
    const host = import.meta.env.SSR
      // eslint-disable-next-line no-undef
      ? useSSRContext().req.headers.host
      : window.location.host
    useHead({
      link: [
        // We use route.path since we don't use query parameters
        { rel: 'canonical', href: `https://${host}${route.path}` }
      ]
    })
  },
  methods: {
    capitalize (str) {
      return `${str.charAt(0).toUpperCase()}${str.slice(1)}`
    }
  }
}
</script>

<style lang="stylus">
body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Fira Sans', sans-serif;
  font-size: 15px;
  background-color: lighten(#eceef1, 30%);
  margin: 0;
  padding: 0;
  color: #2E495E;
  overflow-y: scroll;
}

a {
  color: #2E495E;
  text-decoration: none;
}

.header {
  background-color: #2E495E;
  z-index: 999;
  height: 55px;

  .inner {
    max-width: 800px;
    box-sizing: border-box;
    margin: 0px auto;
    padding: 15px 5px;
  }

  a {
    color: #fff;
    line-height: 24px;
    transition: color 0.15s ease;
    display: inline-block;
    vertical-align: middle;
    font-weight: 300;
    letter-spacing: 0.075em;
    margin-right: 1.8em;

    &:hover {
      color: #fff;
    }

    &.router-link-active, &.nuxt-link-active {
      color: #fff;
      font-weight: 600;
    }

    &:nth-child(6) {
      margin-right: 0;
    }
  }

  .github {
    color: #fff;
    font-size: 0.9em;
    margin: 0;
    float: right;
  }
}

.logo {
  width: 24px;
  margin-right: 10px;
  display: inline-block;
  vertical-align: middle;
}

.view {
  max-width: 800px;
  margin: 0 auto;
  position: relative;
}

.appear-active {
  transition: opacity 0.4s ease;
}

.page-enter-active, .page-leave-active {
  transition: all 0.2s ease;
}

.appear, .page-enter, .page-leave-active {
  opacity: 0;
}

@media (max-width: 860px) {
  .header .inner {
    padding: 15px 30px;
  }
}

@media (max-width: 600px) {
  .header {
    .inner {
      padding: 15px;
    }

    a {
      margin-right: 1em;
    }

    .github {
      display: none;
    }
  }
}
</style>
