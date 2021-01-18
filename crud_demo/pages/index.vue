<template>
  <div class="container">
    <!-- 搜索框 -->
    <Search @on-search="searchUserInfo"></Search>
    <!-- 主要内容 -->
    <div class="main-card">
      <Button class="add-btn" @click="addBtn">
        <Icon class="add-icon" type="md-add" />
      </Button>
      <!-- 表格 -->
      <Table height="450" :columns="columns" :data="data">
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
      :userInfo="userInfo"
      :addOrEdit="addoredit"
      @on-add="addUserInfo"
      @on-edit="editUserInfo"
    ></AEModal>
  </div>
</template>

<script>
import Search from '@/components/Search'
import AEModal from '@/components/AEModal'
export default {
  components: {
    Search,
    AEModal,
  },
  data() {
    return {
      newIndex: '',
      addoredit: false,
      modalState: false,
      userInfo: { name: '', age: '', address: '' },
      userSearch: { name: '', age: '', address: '' },
      columns: [
        {
          title: '姓名',
          key: 'name',
        },
        {
          title: '年龄',
          key: 'age',
        },
        {
          title: '地址',
          key: 'address',
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
          age: '18',
          address: '湘雅居24号',
        },
        {
          name: '李桥',
          age: '24',
          address: '九号公馆501号120室',
        },
        {
          name: '赵倩',
          age: '30',
          address: '篱笆小院201弄',
        },
        {
          name: '钱丽丽',
          age: '26',
          address: '海棠花园201号1247室',
        },
      ],
    }
  },
  methods: {
    // ? 拿到搜索组件传过来的参并搜索
    searchUserInfo(userv) {
      this.data = this.data.filter(
        (x) =>
          x.name.includes(userv.name) &&
          x.age.includes(userv.age) &&
          x.address.includes(userv.address)
      )
    },
    // ? 新增按钮
    addBtn() {
      this.modalState = true
      this.addoredit = false
    },
    // ? 拿到弹窗传过来的对象并新增的操作
    addUserInfo(obj) {
      this.data.push(obj)
      this.modalState = false
    },
    // ? 拿到弹窗传过来的对象并保存的操作
    editUserInfo(obj) {
      this.data[this.newIndex] = obj
      this.modalState = false
    },
    // ? 修改按钮
    modifyBtn(row, index) {
      this.modalState = true
      this.addoredit = true
      this.userInfo = row
      this.newIndex = index
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
