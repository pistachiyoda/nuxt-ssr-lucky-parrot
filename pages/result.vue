<template>
  <div id="app" :style="{ backgroundImage: 'url(' + path + ')' }">
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
      <h2>{{ parrot_name }}</h2>
      <img class="my-1" alt="parrot" :src="path" />
      <a :href="tweet_url" class="btn btn-info btn-lg w-100 my-1">Tweet</a>
      <button
        type="button"
        class="btn btn-warning btn-lg w-100 mt-1"
        @click="show_random_parrot"
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
export default {
  name: 'App',
  data() {
    return {
      parrotArray: [],
      path: '',
      parrot_name: '',
    }
  },
  computed: {
    tweet_url() {
      const parrotParams = new URLSearchParams()
      const urlParams = new URLSearchParams()
      parrotParams.append('parrot_name', this.parrot_name)
      parrotParams.append('path', this.path)
      urlParams.append(
        'text',
        'http://localhost:3000/?' + parrotParams.toString()
      )
      const tweetUrl =
        'https://twitter.com/intent/tweet?' + urlParams.toString()
      return tweetUrl
    },
  },
  watch: {
    '$route.query': 'get_parrot_from_param',
  },
  mounted() {
    this.get_all_parrot()
  },
  methods: {
    async get_all_parrot() {
      this.parrotArray = (await (await fetch('/list.txt')).text()).split('\n')
      this.get_parrot_from_param()
    },
    show_random_parrot() {
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
    get_parrot_from_param() {
      const params = new URLSearchParams(this.$route.query)
      this.parrot_name = params.get('parrot')
      this.parrotArray.forEach((e) => {
        if (e.includes(this.parrot_name)) this.path = e
      })
    },
    get_random_int(max) {
      return Math.floor(Math.random() * Math.floor(max))
    },
    reset() {
      this.is_selected = false
      this.parrot_name = ''
      this.path = ''
    },
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
