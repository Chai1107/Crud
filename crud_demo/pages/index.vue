/* eslint-disable no-unused-expressions */
<template>
  <div class="container">
    <!-- 搜索框 -->
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
    </div>
    <!-- 主要内容 -->
    <div class="main-card">
      <Button class="add-btn" @click="addBtn()">
        <Icon class="add-icon" type="md-add" />
      </Button>
      <!-- 新增弹窗 -->
      <Modal
        v-model="modalAdd"
        ok-text="添加"
        cancel-text="取消"
        @on-ok="addModal"
      >
        <p class="input-title">姓名</p>
        <Input v-model="userAdd.name" placeholder="请输入姓名" clearable />
        <p class="input-title">年龄</p>
        <Input
          v-model="userAdd.age"
          type="number"
          placeholder="请输入年龄"
          clearable
        />
        <p class="input-title">地址</p>
        <Input v-model="userAdd.address" placeholder="请输入地址" clearable />
      </Modal>
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
          <div>
            <Button
              type="primary"
              size="small"
              class="modify-btn"
              @click="modifyBtn(row, index)"
            >
              修改
            </Button>
            <Modal
              v-model="editModal"
              title="修改"
              ok-text="保存"
              cancel-text="取消"
              @on-ok="saveBtn(indexx)"
            >
              <p class="input-title">姓名</p>
              <Input
                v-model="userEdit.name"
                placeholder="请输入姓名"
                clearable
              />
              <p class="input-title">年龄</p>
              <Input
                v-model="userEdit.age"
                type="number"
                placeholder="请输入年龄"
              />
              <p class="input-title">地址</p>
              <Input
                v-model="userEdit.address"
                placeholder="请输入地址"
                clearable
              />
            </Modal>
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
export default {
  data() {
    return {
      indexx: '',
      userv: { name: '', age: '', address: '' }, // ? 搜索框中绑定的姓名、年龄、地址
      userAdd: { name: '', age: '', address: '' }, // ? 新增弹窗中绑定的姓名、年龄、地址
      userEdit: { name: '', age: '', address: '' }, // ? 修改弹窗中绑定的姓名、年龄、地址
      modalAdd: false,
      editModal: false,
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
      if (this.userv.name && !this.userv.age && !this.userv.address) {
        return this.data.filter((x) => {
          return x.name.match(this.userv.name)
        })
      } else if (this.userv.age && !this.userv.name && !this.userv.address) {
        return this.data.filter((x) => {
          return x.age.toString().match(this.userv.age)
        })
      } else if (this.userv.address && !this.userv.age && !this.userv.name) {
        return this.data.filter((x) => {
          return x.address.match(this.userv.address)
        })
      } else if (this.userv.name && this.userv.age && !this.userv.address) {
        return this.data.filter((x) => {
          return (
            x.name.match(this.userv.name) &&
            x.age.toString().match(this.userv.age)
          )
        })
      } else if (this.userv.name && this.userv.address && !this.userv.age) {
        return this.data.filter((x) => {
          return (
            x.name.match(this.userv.name) && x.address.match(this.userv.address)
          )
        })
      } else if (this.userv.age && this.userv.address && !this.userv.name) {
        return this.data.filter((x) => {
          return (
            x.age.toString().match(this.userv.age) &&
            x.address.match(this.userv.address)
          )
        })
      } else if (this.userv.name && this.userv.age && this.userv.address) {
        return this.data.filter((x) => {
          return (
            x.name.match(this.userv.name) &&
            x.age.toString().match(this.userv.age) &&
            x.address.match(this.userv.address)
          )
        })
      } else {
        return this.data
      }
    },
  },
  methods: {
    // ? 保存按钮
    saveBtn(index) {
      if (this.userEdit.name && this.userEdit.age && this.userEdit.address) {
        this.data[index].name = this.userEdit.name
        this.data[index].age = this.userEdit.age
        this.data[index].address = this.userEdit.address
        this.$Message.success('修改成功')
      } else {
        this.$Message.error('修改失败，内容不能为空')
      }
    },
    // ? 新增按钮
    addBtn() {
      this.modalAdd = true
      this.userAdd.name = ''
      this.userAdd.age = ''
      this.userAdd.address = ''
    },
    // ? 添加弹窗中的添加按钮
    addModal() {
      if (this.userAdd.name && this.userAdd.age && this.userAdd.address) {
        this.data.push({
          name: this.userAdd.name,
          age: Number(this.userAdd.age),
          address: this.userAdd.address,
        })
        this.$Message.success('添加成功')
      } else {
        this.$Message.error('添加失败,内容不能为空！')
      }
      this.userAdd.age = ''
      this.userAdd.name = ''
      this.userAdd.address = ''
    },
    // ?修改按钮
    modifyBtn(row, index) {
      this.editModal = true
      this.userEdit.name = row.name
      this.userEdit.age = row.age
      this.userEdit.address = row.address
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
