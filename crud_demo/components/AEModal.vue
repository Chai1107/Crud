<template>
  <div>
    <Modal v-model="open" :closable="false" :mask-closable="false">
      <Form
        ref="userAdd"
        :model="userInfoInput"
        :rules="userRules"
        :label-width="80"
      >
        <FormItem label="姓名" prop="name" class="input-title">
          <Input
            v-model="userInfoInput.name"
            placeholder="请输入姓名"
            clearable
          />
        </FormItem>
        <FormItem label="年龄" prop="age" class="input-title">
          <Input
            v-model="userInfoInput.age"
            type="number"
            placeholder="请输入年龄"
          />
        </FormItem>
        <FormItem label="地址" prop="address" class="input-title">
          <Input
            v-model="userInfoInput.address"
            placeholder="请输入地址"
            clearable
          />
        </FormItem>
        <FormItem>
          <Button @click="addModalClose">取消</Button>
          <Button v-if="!addOrEdit" @click="addModal('userAdd')">添加</Button>
          <Button v-else @click="saveModal('userAdd')">保存</Button>
        </FormItem>
      </Form>
      <div slot="footer"></div>
    </Modal>
  </div>
</template>

<script>
export default {
  name: 'AEModal',
  props: {
    value: { type: Boolean, default: false },
    addOrEdit: { type: Boolean, default: false },
    userInfo: {
      type: Object,
      default: () => {
        return {}
      },
    },
  },
  data() {
    return {
      open: false,
      inputUser: { name: '', age: '', address: '' },
      userRules: {
        name: [{ required: true, message: '姓名不能为空', trigger: 'blur' }],
        age: [{ required: true, message: '年龄不能为空', trigger: 'blur' }],
        address: [{ required: true, message: '地址不能为空', trigger: 'blur' }],
      },
    }
  },
  computed: {
    userInfoInput() {
      if (this.addOrEdit) {
        return this.userInfo
      } else {
        return this.inputUser
      }
    },
  },
  watch: {
    value(val) {
      this.open = val
    },
  },
  methods: {
    addModalClose() {
      this.open = false
      this.$emit('input', false)
    },
    addModal(userAdd) {
      this.$refs[userAdd].validate((valid) => {
        if (valid) {
          this.$emit('on-add', this.inputUser)
          this.$Message.success('添加成功')
        } else {
          this.$Message.error('添加失败,内容不能为空！')
        }
      })
    },
    saveModal(userAdd) {
      this.$refs[userAdd].validate((valid) => {
        if (valid) {
          this.$emit('on-edit', this.userInfo)
          this.$Message.success('保存成功')
        } else {
          this.$Message.error('保存失败,内容不能为空！')
        }
      })
    },
  },
}
</script>

<style scoped>
@import url('../assets/main');
</style>
