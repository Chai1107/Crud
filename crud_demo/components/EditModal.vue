<template>
  <div>
    <Button
      type="primary"
      size="small"
      class="modify-btn"
      @click="modifyBtn(row, index)"
    >
      修改
    </Button>
    <Modal v-model="editModal" title="修改">
      <Form
        ref="userEdit"
        :model="userEdit"
        :rules="userRules"
        :label-width="80"
      >
        <FormItem label="姓名" prop="name">
          <Input v-model="userEdit.name" placeholder="请输入姓名" clearable />
        </FormItem>
        <FormItem label="年龄" prop="age">
          <Input
            v-model="userEdit.age"
            type="number"
            placeholder="请输入年龄"
          />
        </FormItem>
        <FormItem label="地址" prop="address">
          <Input
            v-model="userEdit.address"
            placeholder="请输入地址"
            clearable
          />
        </FormItem>
        <FormItem>
          <Button @click="cancelBtn('userEdit')">取消</Button>
          <Button @click="saveBtn('userEdit', indexx)">保存</Button>
        </FormItem>
      </Form>
      <div slot="footer"></div>
    </Modal>
  </div>
</template>

<script>
export default {
  name: 'EditModal',
  // eslint-disable-next-line vue/require-default-prop
  props: { row: Object, index: Number, listt: Array },
  data() {
    return {
      editModal: false,
      indexx: '',
      userEdit: { name: '', age: '', address: '' }, // ? 修改弹窗中绑定的姓名、年龄、地址
      userRules: {
        name: [{ required: true, message: '姓名不能为空', trigger: 'blur' }],
        age: [{ required: true, message: '年龄不能为空', trigger: 'blur' }],
        address: [{ required: true, message: '地址不能为空', trigger: 'blur' }],
      },
    }
  },
  methods: {
    // ? 取消按钮
    cancelBtn(userEdit) {
      this.editModal = false
    },
    // ?修改按钮
    modifyBtn(row, index) {
      this.editModal = true
      this.userEdit.name = row.name
      this.userEdit.age = row.age.toString()
      this.userEdit.address = row.address
      this.indexx = index
    },
    // ? 保存按钮
    saveBtn(userEdit, index) {
      this.$refs[userEdit].validate((valid) => {
        if (valid) {
          this.listt[index].name = this.userEdit.name
          this.listt[index].age = this.userEdit.age
          this.listt[index].address = this.userEdit.address
          this.$Message.success('修改成功')
          this.editModal = false
        } else {
          this.$Message.error('修改失败，内容不能为空')
        }
      })
    },
  },
}
</script>

<style scoped>
@import url('../assets/main');
</style>
