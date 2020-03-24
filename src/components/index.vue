<template>
  <div id="phonebook">
    <h1>电话本</h1>
    <div class="head">
      <el-row :gutter="10">
        <el-col :span="7">
          <el-input
            @keyup.enter.native="addContacts"
            autofocus
            v-model="contactsInfo.name"
            placeholder="姓名"
          ></el-input>
        </el-col>
        <el-col :span="7">
          <el-input
            @keyup.enter.native="addContacts"
            v-model="contactsInfo.number"
            placeholder="电话号码"
          ></el-input>
        </el-col>
        <el-col :span="7">
          <el-input @keyup.enter.native="addContacts" v-model="contactsInfo.note" placeholder="备注"></el-input>
        </el-col>
        <el-col :span="3">
          <el-button type="primary" @click.prevent="addContacts" style="width:100%" class="btn">添加</el-button>
        </el-col>
      </el-row>
    </div>
    <div class="body">
      <el-table :data="tableData" style="width: 100%">
        <el-table-column label="序号" width="50">
          <template slot-scope="scope">{{scope.$index + 1}}</template>
        </el-table-column>
        <el-table-column prop="name" label="姓名" width="180"></el-table-column>
        <el-table-column prop="number" label="电话" width="180"></el-table-column>
        <el-table-column prop="note" label="备注" width="380"></el-table-column>
        <el-table-column label="操作" width="168">
          <template slot-scope="scope">
            <el-button
              type="primary"
              icon="el-icon-edit"
              @click.prevent="editItem(scope.row,scope.$index)"
              circle
            ></el-button>
            <el-button
              type="danger"
              icon="el-icon-delete"
              circle
              @click.prevent="delInfo(scope.$index)"
            ></el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-dialog title="编辑联系人" :visible.sync="dialogVisible" width="30%">
        <div>
          <el-form ref="form" :model="editInfo" label-width="80px">
            <el-form-item label="姓名">
              <el-input v-model="editInfo.name"></el-input>
            </el-form-item>
            <el-form-item label="电话">
              <el-input v-model="editInfo.number"></el-input>
            </el-form-item>
            <el-form-item label="备注">
              <el-input v-model="editInfo.note"></el-input>
            </el-form-item>
          </el-form>
        </div>
        <span slot="footer" class="dialog-footer">
          <el-button @click="dialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="confirm">确 定</el-button>
        </span>
      </el-dialog>
    </div>
  </div>
</template>
<script>
import Vue from "vue";
export default {
  name: "phonebook",
  data() {
    return {
      contactsInfo: {
        //添加联系人
        name: "",
        number: "",
        note: ""
      },
      tableData: [
        {
          name: "张三",
          number: "13323332333",
          note: "法外狂徒"
        }
      ],
      dialogVisible: false,
      editInfo: {
        name: "",
        number: "",
        note: ""
      },
      saveIndex: 0
    };
  },
  methods: {
    //添加数据
    addContacts() {
      //数据校验
      if (!this.contactsInfo.name) {
        this.$message({
          duration: 600,
          message: "请输入姓名",
          type: "warning"
        });
        return;
      }

      if (!this.contactsInfo.number) {
        this.$message({
          duration: 600,
          message: "请输入电话",
          type: "warning"
        });
        return;
      }
      //验证电话号
      if (!/^1[3456789]\d{9}$/.test(this.contactsInfo.number)) {
        this.$message({
          duration: 600,
          message: "请输入正确的电话",
          type: "warning"
        });
        return;
      }
      //不进行备注时自动填写无
      if (!this.contactsInfo.note) {
        this.contactsInfo.note = "无";
        this.tableData.push(this.contactsInfo);
        this.contactsInfo = {
          //清空输入框
          name: "",
          number: "",
          note: ""
        };
        return;
      }
      this.tableData.push(this.contactsInfo);
      this.contactsInfo = {
        //清空输入框
        name: "",
        number: "",
        note: ""
      };
    },
    //删除数据
    delInfo(idx) {
      this.$confirm("确认删除？")
        .then(() => {
          this.tableData.splice(idx, 1);
        })
        .catch(() => {});
    },
    //修改数据
    editItem(item, idx) {
      this.saveIndex = idx;
      this.editInfo = {
        name: item.name,
        number: item.number,
        note: item.note
      };
      this.dialogVisible = true;
    },
    confirm() {
      Vue.set(this.tableData, this.saveIndex, this.editInfo);
      this.dialogVisible = false;
    }
  }
};
</script>
<style  scoped>
</style>