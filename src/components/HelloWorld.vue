<template lang='pug'>
  .wrapper
    .targets
      .target
        |Character1
        .targetCharacter
          .server
            p server
            select(v-model='character1.server')
              option(v-for='server in servers' :value='server') {{server}}
          .name
            p First name
            input(type='text' v-model='character1.firstName')
          .name
            p Last name
            input(type='text' v-model='character1.lastName')
      .target
        |Character2
        .targetCharacter
          .server
            p server
            select(v-model='character2.server')
              option(v-for='server in servers' :value='server') {{server}}
          .name
            p First name
            input(type='text' v-model='character2.firstName')
          .name
            p Last name
            input(type='text' v-model='character2.lastName')

    .search
      button.button(@click='onClickButton')
        |Search!!
    .twitter
      a.twitterButton(:href="twitterUrl" target="_blank") <i class="fab fa-twitter"></i> Tweet!

    .generateTargets
      .generateTarget
        template(v-if='character1.searchError')
          p
            |Character1
          p.error character is not found.
        template(v-else-if='character1.searching')
          p
            |Character1 searching...
          img(src="https://media.giphy.com/media/sSgvbe1m3n93G/giphy.gif")
        template(v-else-if='!!character1.data')
          p
            |Character1
          FriendCard(:friend='character1.data')
      .generateTarget
        template(v-if='character2.searchError')
          p
            |Character2
          p.error character is not found.
        template(v-else-if='character2.searching')
          p
            |Character2 searching...
          img(src="https://media.giphy.com/media/sSgvbe1m3n93G/giphy.gif")
        template(v-else-if='character2.data')
          p
            |Character2
          FriendCard(:friend='character2.data')


    .result(v-if='character1.data && character2.data && !character1.searchError&& !character2.searchError')
      template(v-if='friendFinding')
        img(src="https://media.giphy.com/media/sSgvbe1m3n93G/giphy.gif")
      template(v-else)
        .resultInfo
          p We are common friends!
          p {{friendsFriendList.length}} persons
          p.error(v-if='character1.data && character1.friends.length <= 0') {{character1.data.Name}} has no friends or can't access friend list.
          p.error(v-if='character2.data && character2.friends.length <= 0') {{character2.data.Name}} has no friends or can't access friend list.
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
  },
  data() {
    return {
      character1: {
        server: '',
        firstName: '',
        lastName: '',
        data: null,
        friends: [],
        searchError: false,
        searching: false,
      },
      character2: {
        server: '',
        firstName: '',
        lastName: '',
        data: null,
        friends: [],
        searchError: false,
        searching: false,
      },
      friendsFriendList: [],
      friendFinding: false,
      servers: ["Adamantoise","Aegis","Alexander","Anima","Asura","Atomos","Bahamut","Balmung","Behemoth","Belias","Brynhildr","Cactuar","Carbuncle","Cerberus","Chocobo","Coeurl","Diabolos","Durandal","Excalibur","Exodus","Faerie","Famfrit","Fenrir","Garuda","Gilgamesh","Goblin","Gungnir","Hades","Hyperion","Ifrit","Ixion","Jenova","Kujata","Lamia","Leviathan","Lich","Louisoix","Malboro","Mandragora","Masamune","Mateus","Midgardsormr","Moogle","Odin","Omega","Pandaemonium","Phoenix","Ragnarok","Ramuh","Ridill","Sargatanas","Shinryu","Shiva","Siren","Tiamat","Titan","Tonberry","Typhon","Ultima","Ultros","Unicorn","Valefor","Yojimbo","Zalera","Zeromus","Zodiark","Spriggan","Twintania"],
      twitterUrl: "http://twitter.com/share?url=https://nanarya.github.io/ffxiv-fff/&text=FF14 Friend's Friend Finder&hashtags=FF14FFF",
    }
  },
  methods: {
    onClickButton () {
      // console.log( this.character1LastName )
      this.resetCharacter(this.character1)
      this.resetCharacter(this.character2)
      this.getCharacter(this.character1)
      this.getCharacter(this.character2)
      this.friendFinding = true
    },
    resetCharacter (character) {
      character.data = {}
      character.friends = []
      character.searchError = false
      character.searching = false
      this.friendsFriendList = []
      this.twitterUrl = "http://twitter.com/share?url=https://nanarya.github.io/ffxiv-fff/&text=FF14 Friend's Friend Finder&hashtags=FF14FFF"
    },
    getCharacter (character) {
      character.searching = true
      // console.log(`https://xivapi.com/character/search?name=${character.firstName}+${character.lastName}&server=${character.server}`)
      fetch(`https://xivapi.com/character/search?name=${character.firstName}+${character.lastName}&server=${character.server}`, { mode: 'cors' })
        .then(response => response.json())
        .then(data => this.setCharacterData(character, data.Results[0]))
    },

    setCharacterData (character, data) {
      // name check
      // console.group()
      // console.log(data)
      // console.log(!!data)
      // console.groupEnd()
      if(!!data && `${character.firstName} ${character.lastName}` == data.Name) {
        character.data = data
        if(!this.character1.searchError && !this.character2.searchError) {
          this.findFriends(character)
        }
      } else {
        character.searchError = true
      }
      character.searching = false
    },
    findFriends (character) {
      fetch(`https://xivapi.com/character/${character.data.ID}?data=FR`, { mode: 'cors' })
        .then(response => response.json())
        .then(data => this.setFriends(character, data.Friends))
      // console.log(this.character1.friends)
    },
    setFriends (character, friends) {
      character.friends = friends
      if(this.character1.friends.length && this.character2.friends.length || friends.length == 0) {
        this.createFriendsFriendList()
      }
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

      this.friendFinding = false
      if(this.friendsFriendList.length > 0) {
        this.twitterUrl = encodeURI(`http://twitter.com/share?url=https://nanarya.github.io/ffxiv-fff/&text=${this.character1.data.Name} and ${this.character2.data.Name} have ${this.friendsFriendList.length} common friends! | FF14 FFF&hashtags=FF14FFF`)
      }
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
  @media screen and (max-width: 1000px)
    flex-flow column wrap
    text-align left

.target
  & + &
    @media screen and (max-width: 1000px)
      margin-top 20px


.targetCharacter
  display flex
  flex-flow row nowrap
  margin-top 10px
  @media screen and (max-width: 1000px)
    flex-flow column wrap

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

.twitter
  margin-bottom 20px
.twitterButton
  text-decoration none
  padding 5px 10px
  background-color #1DA1F2
  color #ffffff
  font-weight bold
  border-radius 5px
  &:hover
    opacity 0.6


.generateTargets
  display flex
  flex-flow row nowrap
  justify-content space-between
  @media screen and (max-width: 1000px)
    flex-flow column wrap

.name
  margin-left 1em
  @media screen and (max-width: 1000px)
    margin-left 0
.result
  border 1px solid #cccccc
  padding 20px

.resultInfo
  margin 20px 0

.error
  color #ff0000
  font-weight bold

.friends
  display flex
  flex-flow row wrap
  justify-content space-between
  list-style none
  li
    @media screen and (max-width: 1000px)
      min-width auto 
      width 100%
      box-sizing border-box
      margin auto

</style>
