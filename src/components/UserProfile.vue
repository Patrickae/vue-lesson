<template>
<div class='user-profile'>
  <div class='user-profile__user-panel'>
    <h1 class='user-profile__username'>@{{user.username}}</h1>
    <div class='user-profile__admin-badge' v-if='user.isAdmin'>
      Admin
    </div>
    <div class='user-profile__follower-count'>
      <strong>Followers: </strong>{{followers}}
    </div>
    <form class='user-profile__create-twoot' @submit.prevent="createNewTwoot" :class="{'--exceeded': newTwootCharacterCount > 180}">
      <label for='newTwoot'><strong>New Twoot: </strong>{{newTwootCharacterCount}}/180</label>
      <textarea id='newTwoot' rows='4' v-model='newTwootContent'/>
      <div class='user-profile__create-twoot-type'>
        <label for='new-twoot-type'>Type: </label>
        <select id='newTwootType' v-model="newTwootType">
          <option :value='option.value' v-for="(option, index) in twootTypes" :key='index'>
            {{option.name}}
          </option>
        </select>
      </div>
      <button>twoot!</button>
    </form>
  </div>
  <div class='user-profile_twoots-wrapper'>
    <TwootItem 
      v-for="twoot in user.twoots" 
      :key="twoot.id" 
      :twoot="twoot" 
      :username="user.username" 
      @favorite='toggleFavorite'
    />
  </div>
</div>
</template>

<script>
import TwootItem from './TwootItem'
export default {
  name: 'UserProfile',
  components: {TwootItem},
  data() {
    return {
      newTwootContent: '',
      newTwootType: 'Instant',
      twootTypes: [
        {value: 'draft', name: 'Draft'},
        {value: 'instant', name: 'Instant Twoot'}
      ],
      followers: 0,
      user: {
        id: 1,
        username: '_andypatrick',
        firstName: 'Andy',
        lastName: 'Patrick',
        email: 'andyepatrick@gmail.com',
        isAdmin: true,
        twoots: [
          {id: 1, content: 'hello'},
          {id: 2, content: 'world'},
          {id: 3, content: 'goodbye'}
        ]
      }
    }
  },
  watch: {
    followers(newCount, oldCount) {
      if (oldCount < newCount) {
        console.log(`${this.user.username} has gained a follower`)
      }
    }
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`
    },
    newTwootCharacterCount(){
      return this.newTwootContent.length
    }
  },
  methods: {
    followUser() {
      this.followers ++
    },
    toggleFavorite(id) {
      console.log(`favorited tweet ${id}`)
    },
    createNewTwoot() {
      if (this.newTwootContent && this.newTwootType == 'instant') {
        this.user.twoots.unshift(
          {
            id: this.user.twoots.length + 1,
            content: this.newTwootContent
          }
        )
      }
      this.newTwootContent = '';
    }
  },
  mounted(){
    this.followUser();
  }
}
</script>

<style scoped lang='scss'>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  width: 100%;
  padding: 50px 5%;
  &__user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #DFE3E8;
  }
  &__admin-badge {
    background: cornflowerblue;
    color: white;
    border-radius: 5px;
    margin-right: auto;
    padding: 0 10px;
    font-weight: bold;
  }
  &__create-twoot {
    display: flex;
    flex-direction: column;
    border-top: 1px solid #ccc;
    padding: 10px;
    &.--exceeded {
     color: red;
    }
  }
}
</style>
