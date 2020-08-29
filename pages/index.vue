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
      <p v-if="is_selected" class="mb-2">Today's your lucky parrots is...</p>
      <p v-else class="mb-0">Choose today's your lucky parrot</p>
      <div v-if="!is_selected" class="parrots d-flex my-4">
        <img
          v-for="(src, index) in random_parrots_list"
          :key="index"
          class="random_parrots"
          :src="src"
        />
      </div>

      <button
        v-show="!is_selected"
        type="button"
        class="btn btn-info btn-lg w-100"
        @click="show_random_parrot"
      >
        <div class="choose_btn">
          <div>Choose</div>
          <div>today's Parrot</div>
        </div>
      </button>
      <div v-if="!is_selected" class="parrots d-flex my-4">
        <img
          v-for="(src, index) in reverse_random_parrots_list"
          :key="index"
          class="random_parrots"
          :src="src"
        />
      </div>
      <h2>{{ parrot_name }}</h2>
      <img v-show="is_selected" class="my-1" alt="parrot" :src="path" />
      <a
        v-show="is_selected"
        :href="tweet_url"
        class="btn btn-info btn-lg w-100 my-1"
        >Tweet</a
      >
      <button
        v-show="is_selected"
        type="button"
        class="btn btn-warning btn-lg w-100 mt-1"
        @click="show_random_parrot"
      >
        Choose parrot again
      </button>
      <a v-show="is_selected" class="mt-3" @click="reset">Back to Top</a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      is_selected: false,
      path: '/images/parrot.gif',
      parrot_name: '',
      random_parrots_list: [],
      reverse_random_parrots_list: [],
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
        'https://todays-lucky-parrot.netlify.com/?' + parrotParams.toString()
      )
      const tweetUrl =
        'https://twitter.com/intent/tweet?' + urlParams.toString()
      return tweetUrl
    },
  },
  mounted() {
    this.get_ramdom_parrots()
    this.get_reverse_ramdom_parrots()
    this.set_params_data()
  },
  methods: {
    async show_random_parrot() {
      const parrotArray = await this.get_all_parrot()
      const path = parrotArray[this.get_random_int(parrotArray.length)]
      this.path = path
      this.parrot_name = path.slice(8, -4)
      this.is_selected = true
    },
    async get_ramdom_parrots() {
      const parrotArray = await this.get_all_parrot()
      for (let i = 0; i < 9; i++) {
        this.random_parrots_list.push(
          parrotArray[this.get_random_int(parrotArray.length)]
        )
      }
      this.get_reverse_ramdom_parrots()
    },
    get_reverse_ramdom_parrots() {
      for (let i = 0; i <= 8; i++) {
        this.reverse_random_parrots_list[i] = this.random_parrots_list[8 - i]
      }
    },
    async get_all_parrot() {
      const response = await fetch('/list.txt')
      const parrotList = await response.text()
      const parrotArray = parrotList.split('\n')
      return parrotArray
    },
    get_random_int(max) {
      return Math.floor(Math.random() * Math.floor(max))
    },
    reset() {
      this.is_selected = false
      this.parrot_name = ''
      this.path = '/images/parrot.gif'
    },
    set_params_data() {
      const params = new URLSearchParams(location.search)
      if (params.get('parrot_name') != null) {
        this.parrot_name = params.get('parrot_name')
        this.path = params.get('path')
        this.is_selected = true
      }
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
