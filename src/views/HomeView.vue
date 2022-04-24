<template>
  <div class="home">
    <MemoTitle msg="Vue CLIを使ったメモアプリ"/>
    <div v-for="(memo, key) in memos" :key="key">
      {{memo}}
    </div>
    <router-link to="/edit">+</router-link>
  </div>
</template>

<script>
// @ is an alias to /src
import MemoTitle from '@/components/MemoTitle.vue'

export default {
  name: 'HomeView',
  components: {
    MemoTitle
  },
  data () {
    return {
      memos: [
        {
          id: 1,
          memo: '今日はあれ'
        },
        {
          id: 2,
          memo: '昨日はこれ'
        },
        {
          id: 3,
          memo: '明日はそれ'
        }
      ]
    }
  }
}
</script>
