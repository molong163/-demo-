<template>
  <div>
    <userFormDialog
      ref="dialogForm"
      :title="title"
      :is-show.sync="isShow"
      :form-data-list="formDataList"
      :form-params="formParams"
      :form-config="formConfig"
      @btnOk="btnOk"
      @btnCancel="btnCancel"
    />
  </div>
</template>
<script>
import userFormDialog from '@/components/CommonForm/index.vue'

export default {
  components: {
    userFormDialog
  },
  data() {
    return {
      isShow: true,
      formParams: {
        userName: '',
        userLoginName: '',
        userPassword: '',
        userPhone: '',
        userEmali: '',
        roleId: ''
      },
      formDataList: [
        {
          label: '用户昵称',
          prop: 'userName',
          type: 'input',
          rules: [
            { required: true, message: '用户昵称不能为空', trigger: 'blur' }
          ]
        },
        {
          label: '登录账户',
          prop: 'userLoginName',
          type: 'input',
          rules: [
            { required: true, message: '登录账户不能为空', trigger: 'blur' }
          ]
        },
        {
          label: '密码',
          prop: 'userPassword',
          type: 'input',
          rules: [{ required: true, message: '密码不能为空', trigger: 'blur' }]
        },
        {
          label: '用户角色',
          prop: 'roleId',
          type: 'select',
          options: [
            { label: '管理员', value: 1 },
            { label: '店长', value: 2 }
          ],
          rules: [
            { required: true, message: '用户角色不能为空', trigger: 'blur' }
          ]
        },
        {
          label: '手机号',
          prop: 'userPhone',
          type: 'input'
        },
        {
          label: '邮件',
          prop: 'userEmali',
          type: 'input'
        }
      ],
      formConfig: {
        labelWidth: '100px',
        inline: false,
        ref: 'formRef'
      },
      title: ''
    }
  },
  methods: {
    btnOk() {},
    btnCancel() {}
  }
}
</script>
