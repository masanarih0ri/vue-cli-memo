<template>
  <div class="home">
    <MemoTitle msg="Vue CLIを使ったメモアプリ"/>
    <div v-for="(memo, key) in memos" :key="key">
      {{memo}}
    </div>
    <router-link :to="this.newMemoUrl" @click="addMemo">+</router-link>
  </div>
</template>

<script>
// @ is an alias to /src
import MemoTitle from '@/components/MemoTitle.vue'

const STORAGE_KEY = 'memo'

export default {
  name: 'HomeView',
  components: {
    MemoTitle
  },
  data () {
    return {
      memos: [],
      memoId: 1
    }
  },
  mounted () {
    if (localStorage.getItem(STORAGE_KEY)) {
      this.memos = JSON.parse(localStorage.getItem(STORAGE_KEY))
      // メモを追加するときにidが被らないようにする
      this.memoId = Math.max(...this.memos.map(memo => memo.id)) + 1
    }
  },
  computed: {
    maxId () {
      return Math.max(...this.memos.map(memo => memo.id)) + 1
    },
    newMemoUrl () {
      return `/memos/${this.maxId}`
    }
  },
  methods: {
    addMemo () {
      this.memos.push({
        id: this.memoId,
        content: '新規メモ'
      })
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.memos))
      this.todoId++
    }
  }
}
</script>
