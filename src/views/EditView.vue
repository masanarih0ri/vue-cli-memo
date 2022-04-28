<template>
  <div class="about">
    <h1>メモの編集ページ</h1>
    <div class="edit">
      <MemoList :memos="memos" @addMemo="addMemo" />
      <div class="edit_area">
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
import MemoList from '@/components/MemoList.vue'

const STORAGE_KEY = 'memo'

export default {
  name: 'EditView',
  components: {
    MemoList
  },
  data () {
    return {
      memos: [],
      memo: null,
      memoId: 1
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
    },
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
    }
  }
}
</script>
<style scoped>
.edit {
  display: flex;
  max-width: 580px;
  margin: 0 auto;
}

.edit_area {
  margin-left: 12px;
}

.list .current {
  color: #000;
  text-decoration: none;
}
</style>
