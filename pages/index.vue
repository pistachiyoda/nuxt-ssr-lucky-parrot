<template>
  <div id="app" :style="{ backgroundImage: 'url(/images/parrot.gif)' }">
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
      <p class="mb-0">Choose today's your lucky parrot</p>
      <div class="parrots d-flex my-4">
        <img
          v-for="(src, index) in random_parrots_list"
          :key="index"
          class="random_parrots"
          :src="src"
        />
      </div>
      <nuxt-link
        :to="{ path: 'result', query: { parrot: parrot_name } }"
        class="btn btn-info btn-lg w-100"
      >
        <div class="choose_btn">
          <div>Choose</div>
          <div>today's Parrot</div>
        </div></nuxt-link
      >
      <div class="parrots d-flex my-4">
        <img
          v-for="(src, index) in reverse_ramdom_parrots"
          :key="index"
          class="random_parrots"
          :src="src"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      parrotArray: '',
      path: '',
      parrot_name: '',
      random_parrots_list: [],
    }
  },
  computed: {
    reverse_ramdom_parrots(randomParrotsList) {
      const reverseRandomParrotsList = []
      for (let i = 0; i <= 8; i++) {
        reverseRandomParrotsList[i] = this.random_parrots_list[8 - i]
      }
      return reverseRandomParrotsList
    },
  },
  mounted() {
    this.get_all_parrot()
  },
  methods: {
    async get_all_parrot() {
      this.parrotArray = (await (await fetch('/list.txt')).text()).split('\n')
      const randomParrotsList = []
      for (let i = 0; i < 9; i++) {
        randomParrotsList.push(
          this.parrotArray[this.get_random_int(this.parrotArray.length)]
        )
      }
      this.random_parrots_list = randomParrotsList
      this.set_random_parrot()
    },
    get_random_int(max) {
      return Math.floor(Math.random() * Math.floor(max))
    },
    set_random_parrot() {
      const path = this.parrotArray[
        this.get_random_int(this.parrotArray.length)
      ]
      this.path = path
      this.parrot_name = path.slice(8, -4)
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
