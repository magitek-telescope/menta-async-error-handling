<template>
  <div id="app">
    <button type="button" @click="handleClickButton1">
      Promise.resolve()
    </button>
    <button type="button" @click="handleClickButton2">
      普通にエラーをなげる
    </button>
    <button type="button" @click="handleClickButton3">
      try でキャッチしてしまう
    </button>
    <div style="margin-top: 16px;">
      Log: <br>
      <code>
        <pre>{{log}}</pre>
      </code>
    </div>
  </div>
</template>

<script>
import { inspect } from 'util'

export default {
  name: 'app',
  data() {
    return {
      log: null
    }
  },
  methods: {
    async handleClickButton1() {
      const asyncFunc = async () => {
        const asyncFunc2 = async () => {
          return 1
        }
        return asyncFunc2()
      }
      try {
        await asyncFunc()
        this.log += "\n" + '最後まで疎通' + "\n----------"
      } catch(e) {
        console.log(e)
        this.log += "\n" + `catch でハンドリング: (${inspect(e)})` + "\n----------"
      }
    },
    async handleClickButton2() {
      const asyncFunc = async () => {
        const asyncFunc2 = async () => {
          throw new Error('何かしらのエラー')
        }
        return asyncFunc2().catch((err) => {
          throw new Error('何かしらのエラー')
        })
      }
      try {
        await asyncFunc()
        this.log += "\n" + '最後まで疎通' + "\n----------"
      } catch(e) {
        console.log(e)
        this.log += "\n" + `catch でハンドリング: (${inspect(e)})` + "\n----------"
      }
    },
    async handleClickButton3() {
      const asyncFunc = async () => {
        const asyncFunc2 = async () => {
          throw new Error('何かしらのエラー')
        }
        return asyncFunc2().catch((err) => {
          throw new Error('何かしらのエラー')
        })
      }
      try {
        await asyncFunc().catch((e) => {
          this.log += "\n" + `エラーを途中でキャッチ${inspect(e)}`
        })
        this.log += "\n" + '最後まで疎通' + "\n----------"
      } catch(e) {
        console.log(e)
        this.log += "\n" + `catch でハンドリング: (${inspect(e)})` + "\n----------"
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  padding: 16px;
}

pre {
  background: #f0f0f0;
  padding: 0 8px;
  line-height: 2.0;
  border-radius: 2px;
}
</style>
