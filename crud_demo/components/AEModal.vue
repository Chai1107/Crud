<template>
  <div>
    <Modal v-model="open" :closable="false" :mask-closable="false">
      <Form ref="userAdd" :model="abc" :rules="userRules" :label-width="80">
        <FormItem label="姓名" prop="name" class="input-title">
          <Input v-model="abc.name" placeholder="请输入姓名" clearable />
        </FormItem>
        <FormItem label="年龄" prop="age" class="input-title">
          <Input v-model="abc.age" type="number" placeholder="请输入年龄" />
        </FormItem>
        <FormItem label="地址" prop="address" class="input-title">
          <Input v-model="abc.address" placeholder="请输入地址" clearable />
        </FormItem>
        <FormItem>
          <Button @click="addModalClose()">取消</Button>
          <Button v-if="addoredit === 0" @click="addModal('userAdd')"
            >添加</Button
          >
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
  // eslint-disable-next-line vue/require-default-prop
  props: {
    value: { type: Boolean, default: false },
    // eslint-disable-next-line vue/require-default-prop
    addoredit: Number,
    // eslint-disable-next-line vue/require-default-prop
    userinfo: Object,
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
    abc() {
      if (this.addoredit === 1) {
        return this.userinfo
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
      this.$emit('modalClose', false)
    },
    addModal(userAdd) {
      this.$refs[userAdd].validate((valid) => {
        if (valid) {
          this.$emit('add', this.inputUser, false)
          this.$Message.success('添加成功')
        } else {
          this.$Message.error('添加失败,内容不能为空！')
        }
      })
    },
    saveModal(userAdd) {
      this.$refs[userAdd].validate((valid) => {
        if (valid) {
          this.$emit('edit', this.userinfo, false)
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
