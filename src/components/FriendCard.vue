<template lang='pug'>
  .friendCard
    .image
      img(:src='friend.Avatar')
    .text
      p.name {{friend.Name}}
      p
        |{{friend.Server}} / 
        a(target='_blank' :href=`url`)
          |LODESTONE(JP)

</template>

<script>
export default {
  name: 'FriendCard',
  props: {
    friend: Object,
  },
  computed: {
    url () {
      return `https://jp.finalfantasyxiv.com/lodestone/character/${this.friend.ID}/friend/`
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='stylus'>
.friendCard
  border 1px solid #cccccc
  margin 20px 0
  display flex
  flex-flow row nowrap
  align-items center
  min-width 420px
  text-align center
  @media screen and (max-width: 1000px)
    min-width auto 
    width 100%
    text-align left
.image
  font-size 0
.text
  padding 0 20px
.name
  font-size 30px
  line-height 1.4
  @media screen and (max-width: 1000px)
    font-size 20px
</style>
