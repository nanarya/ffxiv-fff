<template lang='pug'>
  .wrapper
    .targets
      .target
        |Character1
        .targetCharacter
          .server
            p server
            select(v-model='character1.server')
              option(value='Garuda') Garuda
              option(value='Ifrit') Ifrit
              option(value='Bahamut') Bahamut
          .name
            p First name
            input(type='text' v-model='character1.firstName')
          .name
            p Last name
            input.name(type='text' v-model='character1.lastName')

      .target
        |Character2
        .targetCharacter
          .server
            p server
            select(v-model='character2.server')
              option(value='Garuda') Garuda
              option(value='Ifrit') Ifrit
              option(value='Bahamut') Bahamut
          .name
            p First name
            input(type='text' v-model='character2.firstName')
          .name
            p Last name
            input.name(type='text' v-model='character2.lastName')

    .search
      button.button(@click='onClickButton')
        |Search!!
    .generateTargets
      .generateTarget
        p
          |Character1
        FriendCard(:friend='character1.data')
      .generateTarget
        p
          |Character2
        FriendCard(:friend='character2.data')


    .result
      img(src="https://media.giphy.com/media/sSgvbe1m3n93G/giphy.gif")
      .resultInfo
        p We are common friends!
        p {{friendsFriendList.length}} persons
      ul.friends
        li(v-for='friend in friendsFriendList')
          FriendCard(:friend='friend')
</template>

<script>
import FriendCard from './FriendCard'
export default {
  name: 'HelloWorld',
  components: {
    FriendCard
  },
  props: {
    // msg: String,
  },
  data() {
    return {
      character1: {
        server: 'Garuda',
        firstName: 'Kid',
        lastName: 'Nanarya',
        data: {},
        friends: [],
        error: false,
      },
      character2: {
        server: 'Garuda',
        firstName: 'Kid',
        lastName: 'Nanarya',
        data: {},
        friends: [],
        error: false,
      },
      friendsFriendList: [],
    }
  },
  methods: {
    onClickButton () {
      // console.log( this.character1LastName )
      this.resetCharacter(this.character1)
      this.resetCharacter(this.character2)
      this.getCharacter(this.character1)
      this.getCharacter(this.character2)
    },
    resetCharacter (character) {
      character.data = {}
      character.friend = []
      character.error = false
      this.friendsFriendList = []
    },
    getCharacter (character) {
      // console.log(`https://xivapi.com/character/search?name=${character.firstName}+${character.lastName}&server=${character.server}`)
      fetch(`https://xivapi.com/character/search?name=${character.firstName}+${character.lastName}&server=${character.server}`, { mode: 'cors' })
        .then(response => response.json())
        .then(data => this.setCharacterData(character, data.Results[0]))
    },

    setCharacterData (character, data) {
      // name check
      if(`${character.firstName} ${character.lastName}` == data.Name) {
        character.data = data
        this.findFriends(character)
      } else {
        character.error = true
      }
    },
    findFriends (character) {
      fetch(`https://xivapi.com/character/${character.data.ID}?data=FR`, { mode: 'cors' })
        .then(response => response.json())
        .then(data => this.setFriends(character, data.Friends))
      // console.log(this.character1.friends)
    },
    setFriends (character, friends) {
      character.friends = friends
      this.createFriendsFriendList()
    },
    createFriendsFriendList () {
      let ids1 = this.character1.friends.map( item => item.ID )
      let ids2 = this.character2.friends.map( item => item.ID )
      let allIds = [...ids1, ...ids2]

      // IDs that exists in both
      let targetIds = Array.from(new Set(
        allIds.filter( item => ids1.includes(item) && ids2.includes(item))
      ))

      this.friendsFriendList = this.character1.friends.filter(
        item => targetIds.includes(item.ID)
      )
      // console.group()
      // console.log(this.friendsFriendList)
      // console.log(duplicatedArr) // [ 'apple', 'pen', 'apple', 'pen' ]
      // console.log(new Set(duplicatedArr)) // Set { 'apple', 'pen' }
      // console.groupEnd()
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='stylus'>
.targets
  display flex
  justify-content space-between
  margin 0 auto
.targetCharacter
  display flex
  flex-flow row nowrap
  margin-top 10px

.search
  margin 40px auto
.button
  width 200px
  height 50px
  background #ff0000
  color #ffffff
  font-size 30px
  border none
  transition 0.2s
  box-shadow 2px 2px 2px rgba(0, 0, 0, 0.3)
  &:hover
    opacity 0.6

.generateTargets
  display flex
  flex-flow row nowrap
  justify-content space-between

.name
  margin-left 1em
.result
  border 1px solid #cccccc

.resultInfo
  margin 20px 0

.friends
  display flex
  flex-flow row wrap
  justify-content space-between
  list-style none
</style>
