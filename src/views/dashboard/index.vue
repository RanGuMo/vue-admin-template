<template>
  <div>
   <Card/>
   <Sale/>
  </div>
</template>

<script>
import Card from './Card'
import Sale from './Sale'
import { mapGetters } from 'vuex'

export default {
  name: 'Dashboard',
  components: {
    Card,
    Sale,
  },
  computed: {
    ...mapGetters([
      'name'
    ])
  }
}
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>
