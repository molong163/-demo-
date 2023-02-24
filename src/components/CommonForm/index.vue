<template>
  <div>
    <el-dialog
      :visible="isShow"
      :title="title"
      :width="width"
      @close="btnCancel"
    >
      <el-form
        :ref="formConfig.ref"
        :label-width="formConfig.labelWidth"
        :inline="formConfig.inline"
        :model="formParams"
      >
        <el-form-item
          v-for="(item, index) in formDataList"
          :key="index"
          :label="item.label"
          :prop="item.prop"
          :rules="item.rules"
        >
          <el-input
            v-if="item.type === 'input'"
            v-model="formParams[item.prop]"
            :placeholder="item.placeholder"
          />
          <el-select
            v-if="item.type === 'select'"
            v-model="formParams[item.prop]"
            style="width: 100%"
            :placeholder="item.placeholder"
          >
            <el-option
              v-for="(opItem, index) in item.options"
              :key="index"
              :label="opItem.label"
              :value="opItem.value"
            />
          </el-select>
          <el-date-picker
            v-if="item.type === 'date'"
            v-model="formParams[item.prop]"
            type="date"
            style="width: 100%"
            :placeholder="item.placeholder"
          />
          <el-radio-group
            v-if="item.type === 'radio'"
            v-model="formParams[item.prop]"
            :placeholder="item.placeholder"
          >
            <el-radio
              v-for="(opItem, index) in item.options"
              :key="index"
              :label="opItem.label"
              :rules="item.rules"
            />
          </el-radio-group>
          <el-input
            v-if="item.type === 'textarea'"
            v-model="formParams[item.prop]"
            type="textarea"
            :placeholder="item.placeholder"
          />
          <el-switch
            v-if="item.type === 'switch'"
            v-model="formParams[item.prop]"
          />
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="btnCancel">取 消</el-button>
        <el-button type="primary" @click="btnOk">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  props: {
    width: {
      type: String,
      default: '30%'
    },
    isShow: {
      type: Boolean,
      default: false
    },
    title: {
      type: String,
      default: ''
    },
    formDataList: {
      type: Array,
      default() {
        return []
      }
    },
    formParams: {
      type: Object,
      default() {
        return {}
      }
    },
    formConfig: {
      type: Object,
      default: () => {
        return {}
      }
    }
  },
  data() {
    return {}
  },
  methods: {
    btnCancel() {
      this.$refs[this.formConfig.ref].resetFields()
      this.$emit('btnCancel', false)
    },
    btnOk() {
      this.$refs[this.formConfig.ref].validate((valid) => {
        if (valid) {
          this.$emit('btnOk', this.formParams)
        } else {
          return false
        }
      })
    }
  }
}
</script>

<style>
</style>
