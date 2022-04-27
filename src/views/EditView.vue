<template>
  <div class="about">
    <h1>メモの編集ページ</h1>
    <!-- TODO あとでcssフレームワークとかで対処する -->
    <div class="edit" style="display: flex; max-width: 500px; margin: 0 auto;">
      <div class="list">
        <ul v-for="(memo, key) in memos" :key="key">
          <li><router-link :to="memoUrl(memo)" exact-active-class="current">{{title(memo)}}</router-link></li>
        </ul>
      </div>
      <div>
        <textarea name="" id="" cols="30" rows="10" v-if="memo" v-model="memo.content"></textarea>
      </div>
    </div>
    <div class="buttons">
      <button @click="updateMemo">編集</button>
      <button @click="deleteMemo">削除</button>
    </div>
  </div>
</template>
<script>
const STORAGE_KEY = 'memo'

export default {
  data () {
    return {
      memos: [],
      memo: null
    }
  },
  beforeRouteUpdate (to, _from, next) {
    this.memo = this.memos.find(memo => memo.id === Number(to.params.id))
    next()
  },
  mounted () {
    if (localStorage.getItem(STORAGE_KEY)) {
      this.memos = JSON.parse(localStorage.getItem(STORAGE_KEY))
      this.memo = this.memos.find(memo => memo.id === Number(this.$route.params.id))
    }
  },
  methods: {
    updateMemo () {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.memos))
    },
    deleteMemo () {
      alert('本当に削除しますか？')
      const index = this.memos.findIndex((memo) => memo.id === this.memo.id)
      this.memos.splice(index, 1)
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.memos))
      this.$router.push({
        name: 'home'
      })
    },
    title (memo) {
      const str = memo.content
      const index = str.indexOf('\n')
      return index === -1 ? str : str.substr(0, index)
    },
    memoUrl (memo) {
      return `/memos/${memo.id}`
    }
  }
}
</script>
<style scoped>
.list .current {
  color: #000;
  text-decoration: none;
}
</style>
