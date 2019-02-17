<template>
  <div class="todolist">
    <h1>To Do List</h1>
    <el-row :gutter="20">
      <el-col :span="22" :offset="1">
        <span class="span">姓名</span>
        <el-input
          style="margin-bottom: 10px"
          placeholder="请输入姓名"
          v-model="nameValue"
          clearable>
        </el-input>
        <span class="span">年龄</span>
        <el-input
          placeholder="请输入年龄"
          v-model="ageValue"
          clearable>
        </el-input>
      </el-col>
    </el-row>
    <el-row style="text-align: right;margin: 10px;">
      <el-col :span="23">
        <el-button type="primary" @click="addData">添加</el-button>
        <el-button type="danger" @click="clearValue">重置</el-button>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="22" :offset="1">
        <el-table
          :data="tableData"
          style="width: 100%">
          <el-table-column label="姓名" prop="name">
          </el-table-column>
          <el-table-column label="年龄" prop="age">
          </el-table-column>
          <el-table-column label="操作" width="200">
            <template slot-scope="scope">
              <el-button
                size="mini"
                type="danger"
                @click="handleDelete(scope.$index, scope.row)">删除
              </el-button>
            </template>
          </el-table-column>
        </el-table>
      </el-col>
      <el-col :span="22" :offset="1">
        <div style="text-align: center; margin-top: 20px;">
          <el-button type="danger" @click="clearAll">删除全部</el-button>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        nameValue: '',
        ageValue: '',
        tableData: [],
        url: '../../../static/json/tableData.json'
      }
    },
    methods: {
      async getTableData() {
        let res = await this.axios.get(this.url)
        let resData = res.data
        this.tableData = resData
      },
      addData() {
        if (this.nameValue && this.ageValue) {
          this.tableData.push({
            name: this.nameValue,
            age: this.ageValue
          });
          this.nameValue = '';
          this.ageValue = ''
        } else {
          this.$message.error('请填写完整信息')
        }
      },
      clearValue() {
        this.nameValue = '';
        this.ageValue = ''
      },
      handleDelete(index, row) {
        this.tableData.splice(index, 1);
        this.$message.success('删除成功')
      },
      clearAll() {
        this.tableData = [];
        this.$message.success('全部删除成功')
      }
    },
    created() {
      this.getTableData()
    }
  }
</script>

<style lang="scss" scoped>
  .todolist {
    h1 {
      text-align: center;
      margin: 20px 0;
    }

    .el-row {
      .el-col {
        .span {
          display: inline-block;
          width: 100px;
          text-align: right;
          margin-right: 10px;
        }

        .el-input {
          display: inline-block;
          width: calc(100% - 120px);
        }
      }
    }
  }
</style>
