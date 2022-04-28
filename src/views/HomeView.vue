<template>
  <div class="home">
    <h1>メモアプリ</h1>
    <MemoList :memos="memos" @addMemo="addMemo" />
  </div>
</template>

<script>
import MemoList from '@/components/MemoList.vue'

const STORAGE_KEY = 'memo'

export default {
  name: 'HomeView',
  components: {
    MemoList
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
  methods: {
    addMemo () {
      this.memos.push({
        id: this.memoId,
        content: '新規メモ'
      })
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.memos))
    }
  }
}
</script>
<style scoped>
.home {
  max-width: 580px;
  margin: 0 auto;
}
</style>
