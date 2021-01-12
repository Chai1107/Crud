/* eslint-disable no-unused-expressions */
<template>
  <div class="container">
    <!-- 搜索框 -->
    <div class="search-card">
      <p class="input-title">姓名</p>
      <Input
        v-if="age || address"
        v-model="name"
        class="search-input"
        placeholder="根据姓名查询"
        clearable
        disabled
      />
      <Input
        v-else
        v-model="name"
        class="search-input"
        placeholder="根据姓名查询"
        clearable
      />
      <p class="input-title">年龄</p>
      <Input
        v-if="name || address"
        v-model="age"
        class="search-input"
        type="number"
        placeholder="根据年龄查询"
        disabled
      />
      <Input
        v-else
        v-model="age"
        class="search-input"
        type="number"
        placeholder="根据年龄查询"
      />
      <p class="input-title">地址</p>
      <Input
        v-if="age || name"
        v-model="address"
        class="search-input"
        placeholder="根据地址查询"
        clearable
        disabled
      />
      <Input
        v-else
        v-model="address"
        class="search-input"
        placeholder="根据地址查询"
        clearable
      />
    </div>
    <!-- 主要内容 -->
    <div class="main-card">
      <Button class="addBtn" @click="modalAdd = true">
        <Icon class="addIcon" type="md-add" />
      </Button>
      <!-- 新增弹窗 -->
      <Modal
        v-model="modalAdd"
        ok-text="添加"
        cancel-text="取消"
        @on-ok="addModal"
      >
        <p class="input-title">姓名</p>
        <Input v-model="addName" placeholder="请输入姓名" clearable />
        <p class="input-title">年龄</p>
        <Input
          v-model="addAge"
          type="number"
          placeholder="请输入年龄"
          clearable
        />
        <p class="input-title">地址</p>
        <Input v-model="addAddress" placeholder="请输入地址" clearable />
      </Modal>
      <!-- 表格 -->
      <Table height="450" :columns="columns" :data="list">
        <template slot="name" slot-scope="{ row, index }">
          <Input
            v-if="editIndex === index"
            v-model="editName"
            class="nameInput"
            clearable
            type="text"
          />
          <span v-else>{{ row.name }}</span>
        </template>
        <template slot="age" slot-scope="{ row, index }">
          <Input
            v-if="editIndex === index"
            v-model="editAge"
            class="ageInput"
            type="number"
          />
          <span v-else>{{ row.age }}</span>
        </template>
        <template slot="address" slot-scope="{ row, index }">
          <Input
            v-if="editIndex === index"
            v-model="editAddress"
            class="addressInput"
            clearable
            type="text"
          />
          <span v-else>{{ row.address }}</span>
        </template>
        <template slot="action" slot-scope="{ row, index }">
          <div v-if="editIndex === index" class="operation">
            <Button class="saveBtn" @click="handleSave(index)">保存</Button>
            <Button class="cancelBtn" @click="editIndex = -1">取消</Button>
          </div>
          <div v-else>
            <Button
              type="primary"
              size="small"
              class="modifyBtn"
              @click="modifyBtn(row, index)"
            >
              修改
            </Button>
            <Button
              class="deleteBtn"
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
      name: '',
      age: '',
      address: '',
      addName: '', // ? 添加弹窗中绑定的姓名
      addAge: '', // ? 添加弹窗中绑定的年龄
      addAddress: '', // ? 添加弹窗中绑定的地址
      editIndex: -1, // ? 当前聚焦的输入框的行数
      editName: '', // ? 第一列输入框，当然聚焦的输入框的输入内容，与 data 分离避免重构的闪烁
      editAge: '', // ? 第二列输入框
      editBirthday: '', // ? 第三列输入框
      editAddress: '', // ? 第四列输入框
      modalAdd: false,
      modalModify: false,
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
    // eslint-disable-next-line vue/return-in-computed-property
    list() {
      if (this.name) {
        return this.data.filter((x) => {
          return x.name.match(this.name)
        })
      } else if (this.age) {
        return this.data.filter((x) => {
          return x.age === Number(this.age)
        })
      } else if (this.address) {
        return this.data.filter((x) => {
          return x.address.match(this.address)
        })
      } else {
        return this.data
      }
    },
  },
  methods: {
    handleSave(index) {
      this.data[index].name = this.editName
      this.data[index].age = this.editAge
      this.data[index].address = this.editAddress
      this.editIndex = -1
    },
    addModal() {
      if ((this.addName, this.addAge, this.addAddress)) {
        this.data.push({
          name: this.addName,
          age: Number(this.addAge),
          address: this.addAddress,
        })
      } else {
        this.$Message.error('添加失败,内容不能为空')
      }
      this.addAge = ''
      this.addName = ''
      this.addAddress = ''
    },
    modifyBtn(row, index) {
      console.log(row.age)
      this.editIndex = index
      this.editName = row.name
      this.editAge = row.age
      this.editAddress = row.address
    },
    remove(index) {
      this.data.splice(index, 1)
    },
  },
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  background: url('../static/bg.jpg');
  background-size: cover;
  padding: 24px;
}
.search-card {
  background-color: rgba(255, 255, 255, 0.9);
  width: 700px;
  border-radius: 6px;
  padding: 8px 16px 8px 24px;
  display: flex;
  position: relative;
  height: 48px;
}
.search-input {
  width: 182px !important;
}
.ivu-input-wrapper {
  width: 150px;
  margin-right: 10px;
}
.ivu-input {
  background-color: rgba(255, 255, 255, 0.9);
}
.main-card {
  background-color: rgb(255, 255, 255);
  width: 700px;
  border-radius: 6px;
  padding: 8px 16px;
  margin-top: 8px;
  display: flex;
  flex-direction: column;
}
.ivu-table-wrapper {
  background-color: rgba(250, 128, 114, 0) !important;
  border: none;
  width: 100%;
  margin-left: -9px;
}
.ivu-table th {
  background-color: rgba(250, 128, 114, 0) !important;
}
.input-title {
  line-height: 34px;
  margin-right: 4px;
}
.ivu-table::before,
.ivu-table::after {
  width: 0;
  height: 0;
}
.ivu-modal {
  width: 200px !important ;
}
.operation {
  display: flex;
}
.modifyBtn {
  width: 40px;
  height: 24px;
  padding: 0;
  background-color: #435a9d;
  color: white;
  border: none;
  margin-right: 5px;
}
.modifyBtn:hover {
  width: 40px;
  height: 24px;
  padding: 0;
  background-color: #5c6fa8;
  color: white;
  border: none;
  margin-right: 5px;
}
.saveBtn {
  width: 40px;
  height: 24px;
  padding: 0;
  border-radius: 3px;
  margin-right: 10px;
  background-color: #435a9d;
  color: white;
  border: none;
}
.saveBtn:hover {
  width: 40px;
  height: 24px;
  padding: 0;
  border-radius: 3px;
  margin-right: 10px;
  background-color: #5c6fa8;
  color: white;
  border: none;
}
.cancelBtn {
  width: 40px;
  height: 24px;
  padding: 0;
  border-radius: 3px;
  background-color: #d4546b;
  color: white;
  border: none;
}
.deleteBtn {
  width: 40px;
  height: 24px;
  padding: 0;
  border-radius: 3px;
  background-color: #d4546b;
  color: white;
  border: none;
}
.deleteBtn:hover {
  width: 40px;
  height: 24px;
  padding: 0;
  border-radius: 3px;
  background-color: #d46b7f;
  color: white;
  border: none;
}
.cancelBtn:hover {
  width: 40px;
  height: 24px;
  padding: 0;
  border-radius: 3px;
  background-color: #d46b7f;
  color: white;
  border: none;
}
.addBtn {
  width: 48px;
  padding: 4px;
  background-color: #435a9d;
  color: white;
  border: none;
}
.addBtn:hover {
  width: 48px;
  padding: 4px;
  background-color: #5c6fa8;
  color: white;
  border: none;
}
.addIcon {
  font-size: 16px;
}
.ageInput {
  width: 64px;
}
.nameInput {
  width: 104px;
}
.addressInput {
  width: 180px;
}
</style>
