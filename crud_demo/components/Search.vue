<template>
  <div class="search-card">
    <p class="input-title">姓名</p>
    <Input
      v-model="userv.name"
      class="search-input"
      placeholder="根据姓名查询"
      clearable
    />
    <p class="input-title">年龄</p>
    <Input
      v-model="userv.age"
      class="search-input"
      type="number"
      placeholder="根据年龄查询"
    />
    <p class="input-title">地址</p>
    <Input
      v-model="userv.address"
      class="search-input"
      placeholder="根据地址查询"
      clearable
    />
    <Button @click="searchBtn">搜索</Button>
  </div>
</template>

<script>
export default {
  name: 'Search',
  data() {
    return {
      userv: { name: '', age: '', address: '' }, // ? 搜索框中绑定的姓名、年龄、地址
    }
  },
  methods: {
    searchBtn() {
      this.$emit(
        'on-search',
        this.userv.name,
        this.userv.age,
        this.userv.address
      )
    },
  },
}
</script>

<style scoped>
@import url('../assets/main');
</style>
