<template>
  <div class="container">
    <div class="common-body">
      <el-form
        ref="baseForm"
        class="base-form"
        :model="baseForm"
        auto-complete="on"
        hide-required-asterisk
      >
        <el-table
          id="singleTable"
          ref="singleTable"
          :max-height="tableHeight"
          :data="baseForm.tableData"
          style="width: 100%"
          border
        >
          <el-table-column
            label="序号"
            type="index"
            min-width="50"
            max-width="60"
            align="center"
          />
          <el-table-column
            min-width="140"
            label="任务主题"
          >
            <template slot-scope="scope">
              <el-form-item
                label=" "
                :prop="'tableData.' + scope.$index + '.missionTitle'"
                :rules="rules.missionTitle"
              >
                <el-input
                  v-model="scope.row.missionTitle"
                  clearable
                  type="textarea"
                  size="mini"
                  placeholder="请输入"
                />
              </el-form-item>
            </template>
          </el-table-column>
          <el-table-column
            fixed="right"
            label="操作"
            width="90"
            prop=""
          >
            <template slot-scope="scope">
              <button
                @click="handleAdd(scope.row, scope.$index)"
              >
                添加
              </button>
              <button
                @click="handleDelete(scope.row, scope.$index)"
              >删除</button>
            </template>
          </el-table-column>
        </el-table>
      </el-form>
      <button
        @click="submit()"
      >确认</button>
    </div>
  </div>
</template>

<script>
// import { getElementHight } from '@/utils'
import { cloneDeep } from 'lodash'
export default {
  name: 'Publish',
  data() {
    return {
      dialogVisible: false,
      tableHeight: 500,
      bipAllLists: [],
      deptBipCodeList: [],
      searchForm: {},
      deptOption: [], // 共享管理组织
      submitLoading: false,
      missionTypeLists: [],
      baseForm: {
        tableData: [{
          missionTitle: '999'
        }]
      },
      rules: {
        missionTitle: [
          { required: true, message: '请选择任务主题！', trigger: 'blur' }
        ]
      }
    }
  },
  mounted() {
    // const vm = this
    // setTimeout(() => {
    //   vm.setTableHeight()
    // }, 50)
    // window.onresize = this.$debounce(() => {
    //   //   console.log('onresize')
    //   vm.setTableHeight()
    // }, 50)
  },
  // 注销window.onresize事件
  destroyed() {
    window.onresize = null
  },
  methods: {
    handleClose() {
      this.dialogVisible = false
    },

    // 设置element 表格高度
    setTableHeight() {
      // this.tableHeight = getElementHight(
      //   'common-app',
      //   30 + 50 + 20 + 36 + 10,
      //   '-'
      // )
    },
    // 提交保存
    submit() {
      this.$refs.baseForm.validate((valid) => {
        if (valid) {
        //   const data = cloneDeep(this.baseForm.tableData)
        }
      })
    },
    // 添加行-复制当前行，往下加一行
    handleAdd(row, index) {
      const data = cloneDeep(this.baseForm.tableData)
      data.splice(+index + 1, 0, row)
      this.baseForm = {
        ...this.baseForm,
        tableData: cloneDeep(data)
      }
    },
    // 删除当前行
    handleDelete(row, index) {
      if (this.baseForm.tableData.length === 1) {
        return this.$message.error('当前为最后一行，不可删除')
      }
      const data = cloneDeep(this.baseForm.tableData)
      data.splice(index, 1)
      this.baseForm = {
        ...this.baseForm,
        tableData: cloneDeep(data)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.btnImgAdd {
  width: 18px;
  height: 18px;
  margin-right: 5px;
  cursor: pointer;
  vertical-align: middle;
}
.btnImg {
  width: 24px;
  height: 24px;
  cursor: pointer;
  vertical-align: middle;
}
::v-deep {
  .el-table .cell {
    overflow: visible;
  }
  .el-form-item__error {
    padding-top: 0;
    margin-top: -1px;
  }
  .common-body .el-form-item {
    margin: 0;
  }
  .el-table .el-table__body th,
  .el-table .el-table__body td {
    padding: 12px 0;
  }
}

.popover-container {
  padding: 24px 20px;
}
</style>
