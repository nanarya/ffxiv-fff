<template lang='pug'>
  .wrapper

    .targetCharacter
      select(v-model='character1.server')
        option(value='Garuda') Garuda
        option(value='Ifrit') Ifrit
        option(value='Bahamut') Bahamut
      input(type='text' v-model='character1.firstName')
      input(type='text' v-model='character1.lastName')
      p
        |{{ character1.data.Name }}
        |ID:{{character1.data.ID}}
        |Server:{{character1.data.Server}}
      img(:src='character1.data.Avatar')

    .targetCharacter
      select(v-model='character1.server')
        option(value='Garuda') Garuda
        option(value='Ifrit') Ifrit
        option(value='Bahamut') Bahamut
      input(type='text' v-model='character2.firstName')
      input(type='text' v-model='character2.lastName')
      p
        |{{ character2.data.Name }}
        |ID:{{character2.data.ID}}
        |Server:{{character2.data.Server}}
      img(:src='character2.data.Avatar')

    .div
      button(@click='onClickButton')
        |Character
  
</template>

<script>
export default {
  name: 'HelloWorld',
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
      }
    }
  },
  methods: {
    onClickButton () {
      // console.log( this.character1LastName )
      this.getCharacter(this.character1)
      this.getCharacter(this.character2)
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
      let arr1 = this.character1.friends
      let arr2 = this.character2.friends
      
      let allFriends = [...arr1, ...arr2]

      let duplicatedArr = allFriends.filter(
        item => arr1.includes(item) && arr2.includes(item)
      )

      console.log(duplicatedArr) // [ 'apple', 'pen', 'apple', 'pen' ]
      console.log(new Set(duplicatedArr)) // Set { 'apple', 'pen' }
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
