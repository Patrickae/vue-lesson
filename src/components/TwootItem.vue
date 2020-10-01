<template>
  <div class='user-profile__twoot' @click="favoriteTwoot(twoot.id)">
    {{twoot.content}}
  </div>
</template>

<script>
export default {
  name: 'TwootItem',
  props: {
    username: String,
    twoot: Object
  },
  methods: {
    favoriteTwoot(id) {
      this.$emit('favorite', id)
    }
  }
}
</script>

<style scoped lang='scss'>
  .user-profile__twoot {
    padding: 30px 10px;
    background-color: #ddd;
    margin-bottom: 5px;
    cursor: pointer;
    border-radius: 5px;
    transition: all 0.15s ease;
    &:hover {
      transform: scale(1.1, 1.1);
    }
  }
</style>