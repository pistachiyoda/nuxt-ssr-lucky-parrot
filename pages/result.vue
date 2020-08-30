<template>
  <div id="app" :style="{ backgroundImage: 'url(' + parrotImgPath + ')' }">
    <div id="overlay"></div>
    <div class="info d-flex">
      <a href="https://cultofthepartyparrot.com/">
        <img src="/images/originalparrot.gif" />
      </a>
      <a href="https://twitter.com/pistachiyoda">
        <i class="fab fa-twitter-square"></i>
      </a>
    </div>
    <div id="content" class="p-3">
      <p class="mb-2">Today's your lucky parrots is..</p>
      <h2>{{ parrotName }}</h2>
      <img class="my-1" alt="parrot" :src="parrotImgPath" />
      <a :href="tweetUrl" class="btn btn-info btn-lg w-100 my-1">Tweet</a>
      <button
        type="button"
        class="btn btn-warning btn-lg w-100 mt-1"
        @click="showRandomParrot"
      >
        Choose parrot again
      </button>
      <nuxt-link to="/" class="mt-3 text-dark" @click="reset"
        >Back to Top</nuxt-link
      >
    </div>
  </div>
</template>

<script>
async function getAllParrot() {
  const parrotArray = (
    await (
      await fetch('https://nuxt-ssr-lucky-parrot.vercel.app/list.txt')
    ).text()
  ).split('\n')
  return parrotArray
}

export default {
  name: 'App',
  async asyncData(context) {
    const parrotArray = await getAllParrot()
    const parrotName = context.query.parrot
    const parrotImgPath = parrotArray.find((e) => e.includes(parrotName))
    return { parrotArray, parrotName, parrotImgPath }
  },
  computed: {
    tweetUrl() {
      const urlParams = new URLSearchParams()
      urlParams.append(
        'text',
        'https://nuxt-ssr-lucky-parrot.vercel.app' + this.$nuxt.$route.fullPath
      )
      const tweetUrl =
        'https://twitter.com/intent/tweet?' + urlParams.toString()
      return tweetUrl
    },
  },
  watch: {
    '$route.query': 'onRegenerate',
  },
  methods: {
    showRandomParrot() {
      const path = this.parrotArray[
        this.get_random_int(this.parrotArray.length)
      ].slice(8, -4)
      this.$router.push({
        path: 'result',
        query: {
          parrot: path,
        },
      })
    },
    async onRegenerate() {
      this.parrotArray = await getAllParrot()
      this.parrotName = this.$route.query.parrot
      this.parrotImgPath = this.parrotArray.find((e) =>
        e.includes(this.parrotName)
      )
    },
    get_random_int(max) {
      return Math.floor(Math.random() * Math.floor(max))
    },
    reset() {
      this.parrotName = ''
      this.parrotImgPath = ''
    },
  },
  head() {
    return {
      title: this.title,
      meta: [
        {
          hid: 'og:title',
          property: 'og:title',
          content: 'Todays lucky parroto is...' + this.parrotName + '!!',
        },
        {
          hid: 'og:url',
          property: 'og:url',
          content:
            'https://nuxt-ssr-lucky-parrot.vercel.app' +
            this.$nuxt.$route.fullPath,
        },
        {
          hid: 'og:image',
          property: 'og:image',
          content:
            'https://nuxt-ssr-lucky-parrot.vercel.app' + this.parrotImgPath,
        },
      ],
    }
  },
}
</script>

<style>
p {
  font-size: 12px;
  text-align: center;
}
img {
  height: 200px;
  width: auto;
}
h2 {
  font-size: 18px;
}
a {
  cursor: pointer;
}
div > a > img {
  width: 70px;
  height: auto;
}
.random_parrots {
  width: 30px;
  height: auto;
}
.choose_btn {
  font-size: 16px;
}
.btn-lg {
  font-size: 10px;
}
.fa-twitter-square {
  color: #1da1f2;
  font-size: 50px;
  vertical-align: bottom;
}
.info {
  position: fixed;
  right: 10px;
  bottom: 10px;
  z-index: 10;
  align-items: flex-end;
}
#app {
  height: 100vh;
  background-position: center;
  padding-top: 100px;
  font-family: 'Press Start 2P';
}
#overlay {
  background-color: rgba(0, 0, 0, 0.5);
  height: 100vh;
  width: 100vw;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1;
}
#content {
  z-index: 10;
  position: relative;
  flex-direction: column;
  display: flex;
  width: 290px;
  margin: auto;
  align-items: center;
  background-color: whitesmoke;
}
@media screen and (min-width: 768px) {
  p {
    font-size: 15px;
  }
  h2 {
    font-size: 30px;
  }
  div > a > img {
    width: 120px;
    height: auto;
  }
  .random_parrots {
    width: 60px;
    height: auto;
  }
  .choose_btn {
    font-size: 30px;
  }
  #app {
    height: 100vh;
    background-position: center;
    padding-top: 100px;
    font-family: 'Press Start 2P';
  }
  #overlay {
    background-color: rgba(0, 0, 0, 0.5);
    height: 100vh;
    width: 100vw;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 1;
  }
  #content {
    z-index: 10;
    position: relative;
    flex-direction: column;
    display: flex;
    width: 600px;
    margin: auto;
    align-items: center;
    background-color: whitesmoke;
  }
  img {
    height: 400px;
    width: auto;
  }
  .fa-twitter-square {
    color: #1da1f2;
    font-size: 80px;
    vertical-align: bottom;
  }
  .info {
    position: fixed;
    right: 15px;
    bottom: 15px;
    z-index: 10;
    align-items: flex-end;
  }
}
</style>
