<template>
  <div>
    <Button class="add-btn" @click="addBtn()">
      <Icon class="add-icon" type="md-add" />
    </Button>
    <!-- 新增弹窗 -->
    <Modal v-model="modalAdd" ok-text="添加" cancel-text="取消">
      <Form ref="userAdd" :model="userAdd" :rules="userRules" :label-width="80">
        <FormItem label="姓名" prop="name" class="input-title">
          <Input v-model="userAdd.name" placeholder="请输入姓名" clearable />
        </FormItem>
        <FormItem label="年龄" prop="age" class="input-title">
          <Input v-model="userAdd.age" type="number" placeholder="请输入年龄" />
        </FormItem>
        <FormItem label="地址" prop="address" class="input-title">
          <Input v-model="userAdd.address" placeholder="请输入地址" clearable />
        </FormItem>
        <FormItem>
          <Button @click="addModalClose('userAdd')">取消</Button>
          <Button @click="addModal('userAdd')">添加</Button>
        </FormItem>
      </Form>
      <div slot="footer"></div>
    </Modal>
  </div>
</template>

<script>
export default {
  name: 'AddModal',
  // eslint-disable-next-line vue/require-default-prop
  props: { listt: Array },
  data() {
    return {
      modalAdd: false,
      userAdd: { name: '', age: '', address: '' }, // ? 新增弹窗中绑定的姓名、年龄、地址
      userRules: {
        name: [{ required: true, message: '姓名不能为空', trigger: 'blur' }],
        age: [{ required: true, message: '年龄不能为空', trigger: 'blur' }],
        address: [{ required: true, message: '地址不能为空', trigger: 'blur' }],
      },
    }
  },
  methods: {
    // ? 新增按钮
    addBtn() {
      this.modalAdd = true
      this.userAdd.name = ''
      this.userAdd.age = ''
      this.userAdd.address = ''
    },
    // ? 添加弹窗中的添加按钮
    addModal(userAdd) {
      this.$refs[userAdd].validate((valid) => {
        if (valid) {
          this.listt.push({
            name: this.userAdd.name,
            age: Number(this.userAdd.age),
            address: this.userAdd.address,
          })
          this.$Message.success('添加成功')
          this.modalAdd = false
        } else {
          this.$Message.error('添加失败,内容不能为空！')
        }
      })
    },
    // ? 新增弹窗的取消按钮
    addModalClose(userAdd) {
      this.$refs[userAdd].resetFields()
      this.modalAdd = false
    },
  },
}
</script>

<style scoped>
@import url('../assets/main');
</style>
