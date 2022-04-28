<template>
  <div class="list">
    <ul v-for="(memo, key) in memos" :key="key">
      <li class="list-item">
        <router-link :to="memoUrl(memo)" exact-active-class="current">
          {{ title(memo) }}
        </router-link>
      </li>
    </ul>
    <router-link :to="this.newMemoUrl" @click="$emit('addMemo')">+</router-link>
  </div>
</template>

<script>

export default {
  props: {
    memos: Array
  },
  emits: ['addMemo'],
  computed: {
    maxId () {
      return this.memos.length ? Math.max(...this.memos.map(memo => memo.id)) + 1 : 1
    },
    newMemoUrl () {
      return `/memos/${this.maxId}`
    }
  },
  methods: {
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
.list-item {
  list-style: none;
}

.list .current {
  color: #000;
  text-decoration: none;
}
</style>
