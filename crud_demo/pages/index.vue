<template>
  <div class="container">
    <!-- 搜索框 -->
    <Search @getList="getList"></Search>
    <!-- 主要内容 -->
    <div class="main-card">
      <Button class="add-btn" @click="addBtn">
        <Icon class="add-icon" type="md-add" />
      </Button>
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
            <Button
              type="primary"
              size="small"
              class="modify-btn"
              @click="modifyBtn(row, index)"
            >
              修改
            </Button>
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
    <!-- 弹窗 -->
    <AEModal
      v-model="modalState"
      :userinfo="userInfo"
      :addoredit="addoredit"
      @modalClose="modalClose"
      @add="add"
      @edit="edit"
    ></AEModal>
  </div>
</template>

<script>
import Search from '../components/Search'
import AEModal from '../components/AEModal'
export default {
  components: {
    Search,
    AEModal,
  },
  data() {
    return {
      indexx: '',
      addoredit: 0,
      modalState: false,
      userInfo: { name: '', age: '', address: '' },
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
    // ? 拿到搜索组件传过来的参并赋值
    getList(userName, userAge, userAddress) {
      this.userSearch.name = userName
      this.userSearch.age = userAge
      this.userSearch.address = userAddress
    },
    // ? 关闭弹窗组件
    modalClose(state) {
      this.modalState = state
    },
    // ? 新增按钮
    addBtn() {
      this.modalState = true
      this.addoredit = 0
    },
    // ? 拿到弹窗传过来的对象并新增的操作
    add(obj, state) {
      this.data.push(obj)
      this.modalState = state
    },
    // ? 拿到弹窗传过来的对象并保存的操作
    edit(obj, state) {
      this.data[this.indexx].name = obj.name
      this.data[this.indexx].age = obj.age
      this.data[this.indexx].address = obj.address
      this.modalState = state
    },
    // ? 修改按钮
    modifyBtn(row, index) {
      this.modalState = true
      this.addoredit = 1
      this.userInfo.name = row.name
      this.userInfo.age = row.age.toString()
      this.userInfo.address = row.address
      this.indexx = index
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
