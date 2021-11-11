<template>
  <div class="toggle-row">
    <div class="title" @click="isVisible = !isVisible">{{ item.first_name }}</div>
    <div class="toggle" v-show="isVisible">
      <div class="row">
        <div class="col-1">{{ item.id }}</div>
        <div class="col-1"><img :src="item.avatar" width="60" alt=""></div>
        <div class="col-3">{{ item.email }}</div>
        <div class="col-3">{{ item.first_name }}</div>
        <div class="col-3">{{ item.phone }}</div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'toggleItem',
  props: {
    item: {
      type: Object,
      default: () => []
    }
  },
  data(){
    return {
      isVisible: false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
