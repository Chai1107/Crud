<template>
  <div class="container">
    <!-- 搜索框 -->
    <Search :listt="data" @getList="getList"></Search>
    <!-- 主要内容 -->
    <div class="main-card">
      <AddModal :listt="data"></AddModal>
      <!-- 表格 -->
      <Table height="450" :columns="columns" :data="list">
        <template slot="name" slot-scope="{ row }">
          <span>{{ row.name }}</span>
        </template>
        <template slot="age" slot-scope="{ row }">
          <span>{{ row.age }}</span>
        </template>
        <template slot="address" slot-scope="{ row }">
          <span>{{ row.address }}</span>
        </template>
        <template slot="action" slot-scope="{ row, index }">
          <div class="action">
            <EditModal :row="row" :index="index" :listt="data"></EditModal>
            <Button
              class="delete-btn"
              type="error"
              size="small"
              @click="remove(index)"
            >
              删除
            </Button>
          </div>
        </template>
      </Table>
    </div>
  </div>
</template>

<script>
import Search from '../components/Search'
import AddModal from '../components/AddModal'
import EditModal from '../components/EditModal'
export default {
  components: {
    Search,
    AddModal,
    EditModal,
  },
  data() {
    return {
      userSearch: { name: '', age: '', address: '' },
      columns: [
        {
          title: '姓名',
          slot: 'name',
        },
        {
          title: '年龄',
          slot: 'age',
        },
        {
          title: '地址',
          slot: 'address',
          width: 200,
        },
        {
          title: '操作',
          slot: 'action',
          width: 150,
          align: 'left',
        },
      ],
      data: [
        {
          name: '张黎明',
          age: 18,
          address: '湘雅居24号',
        },
        {
          name: '李桥',
          age: 24,
          address: '九号公馆501号120室',
        },
        {
          name: '赵倩',
          age: 30,
          address: '篱笆小院201弄',
        },
        {
          name: '钱丽丽',
          age: 26,
          address: '海棠花园201号1247室',
        },
      ],
    }
  },
  computed: {
    // ? 查询功能
    list() {
      return this.data.filter(
        (x) =>
          x.name.includes(this.userSearch.name) &&
          x.age.toString().includes(this.userSearch.age) &&
          x.address.includes(this.userSearch.address)
      )
    },
  },
  methods: {
    getList(userName, userAge, userAddress) {
      this.userSearch.name = userName
      this.userSearch.age = userAge
      this.userSearch.address = userAddress
    },
    // ?删除按钮
    remove(index) {
      this.data.splice(index, 1)
    },
  },
}
</script>

<style>
@import url('../assets/main');
</style>
