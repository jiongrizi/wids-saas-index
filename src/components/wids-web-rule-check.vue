<template>
  <div class="custom-wrap">
    <el-row type="flex" class="row-bg" justify="space-between">
      <el-col :span="24">
        <el-row type="flex" class="row-bg" justify="space-between">
          <el-col :span="12">
            <h3>规则检测</h3>
          </el-col>

        </el-row>
      </el-col>
    </el-row>
    <div class="margin-4x"></div>
    <el-row v-loading.fullscreen.lock="loadingPage">
      <el-col :span="24">
        <el-row class="text-align-right">
          <el-button type="success" @click="onClickChangeOrder()">优先级调整</el-button>
          <el-button type="primary" @click="onClickDownloadRule()" :loading="loadingDownloadRule">加载</el-button>

        </el-row>
        <div class="demo-block">
          <el-table :data="tableData" style="width: 100%">
            <el-table-column prop="rule_name" label="规则名称"></el-table-column>
            <el-table-column prop="rule_detail" label="规则描述"></el-table-column>
            <el-table-column prop="status" label="状态">
              <template #default="scope">
                <el-switch v-model="scope.row.status" @change="onChangeRuleStatus(scope.row)" active-value="true"
                  inactive-value="false" />
              </template>
            </el-table-column>

            <el-table-column label="操作" align="right">
              <template #default="scope">

                <el-popover placement="top" width="160" v-model:visible="scope.row.isVisiblePopover">
                  <p>确定删除吗？</p>
                  <div style="text-align: right; margin: 0">
                    <el-button size="mini" type="text" @click="scope.row.isVisiblePopover = false">取消</el-button>
                    <el-button type="primary" size="mini" @click="handleDelete(scope.row)" :loading="loading">确定
                    </el-button>
                  </div>
                  <template #reference>
                    <el-button type="text" size="mini" @click="scope.row.isVisiblePopover = true">删除</el-button>
                  </template>
                </el-popover>
              </template>
            </el-table-column>
            <el-table-column label="优先级" align="right" v-if="!isShowOrder">
              <template #default="scope">
                <el-button type="success" class="icon iconfont iconxiangshang" circle
                  @click="onClickChangeOrderSubmit(scope.$index, scope.row, 'up')" title="上移" :loading="orderLoading" />
                <el-button type="success" class="icon iconfont iconxiangxia" circle
                  @click="onClickChangeOrderSubmit(scope.$index, scope.row, 'down')" title="下移"
                  :loading="orderLoading" />
                <el-button type="success" class="icon iconfont iconzhiding" circle
                  @click="onClickChangeOrderSubmit(scope.$index, scope.row, 'top')" title="置顶"
                  :loading="orderLoading" />
              </template>
            </el-table-column>

          </el-table>
        </div>
      </el-col>

      <el-dialog v-model="dialogDownloadRuleFormVisible" title="加载" :close-on-click-modal="false" width="520px"
        @closed="dialogCloseDownloadRule">
        <el-form class="form-download-rule-dialog" :model="downloadRuleForm" label-position="left" label-width="130px"
          :rules="rules" ref="downloadRuleForm">
          <el-form-item label="加载" key="1" prop="rule_name">
            <el-select v-model="downloadRuleForm.rule_name" placeholder="请选择或输入模糊搜索" filterable>
              <el-option v-for="item in ruleOptions" :key="item.rule_name" :label="item.rule_name" :value="item.rule_name">
              </el-option>
            </el-select>
          </el-form-item>
        </el-form>
        <template #footer class="dialog-footer">
          <el-button @click="dialogDownloadRuleFormVisible = false">取消</el-button>
          <el-button type="primary" @click="onClickDownloadRuleSubmit('downloadRuleForm')" :loading="loading">确定
          </el-button>
        </template>
      </el-dialog>


    </el-row>

  </div>
</template>
<script>
import { mixin, JXAjax } from "../assets/scripts/common";
export default {
  mixins: [mixin],
  data() {
    return {
      loading: false,
      loadingPage: false,
      isShowOrder: true,
      orderLoading: false,
      tableData: [],
      dialogDownloadRuleFormVisible: false,
      loadingDownloadRule: false,
      downloadRuleForm: [],
      ruleOptions: [],
    };
  },
  computed: {
    rules() {
      return {
        rule_name: [
          {
            required: true,
            message: '请选择一个规则',
            trigger: 'change',
          },
        ],
      }
    }
  },
  mounted() {
    this.getData();
    
  },
  methods: {
    getData() {
      var t = this;
<<<<<<< HEAD
      var postUrl = "/wids/waf_get_wids_web_rule_check_list";
=======
      var postUrl = "/sys/waf_get_wids_web_rule_check_list";
>>>>>>> 32b45c66599ebbe10939a1aaf64e015bf985a11f
      var oData = {  };
      JXAjax(
        "post",
        postUrl,
        oData,
        function (response) {
          t.loadingPage = false;
          t.tableData = response.data.message;
          t.tableData.forEach(item => {
            item.isVisiblePopover = false;
          })
        },
        function () {
          t.loadingPage = false;
        },
        "no-message"
      );
    },
    onClickDownloadRule() { 
      var t = this;
      t.loadingDownloadRule = true;
      t.dialogDownloadRuleFormVisible = true;
      JXAjax(
        "post",
        "/sys/waf_get_sys_web_rule_check_list",
        {  },
        function (response) {
          t.loadingDownloadRule = false;
          var _ruleOptions = response.data.message;
          t.ruleOptions = t.getDifference(t.tableData, _ruleOptions)
        },
        function () {
          t.loadingDownloadRule = false;
        },
        "no-message"
      );
    },
    getDifference(arr1, arr2) {
      var newArr = [];
      arr2.forEach(item => {
        var flag = true;
        arr1.forEach(i => {
          if (i.rule_name == item.rule_name) {
            flag = false
          }
        })
        if (flag) {
          newArr.push(item)
        }
      })
      return newArr;
    },
    onClickDownloadRuleSubmit(downloadRuleForm) { 
      var t = this;
<<<<<<< HEAD
      var postUrl = "/wids/waf_load_wids_web_rule_check";
=======
      var postUrl = "/sys/waf_load_wids_web_rule_check";
>>>>>>> 32b45c66599ebbe10939a1aaf64e015bf985a11f
      var oData = { rule_name: t.downloadRuleForm.rule_name };
      
      this.$refs[downloadRuleForm].validate((valid) => {
        if (valid) {
          t.loading = true;
          JXAjax(
            "post",
            postUrl,
            oData,
            function (response) {
              t.loading = false;
              t.dialogDownloadRuleFormVisible = false;
              t.getData();
            },
            function () {
              t.loading = false;
            }
          );
        }
      });
    },
    dialogCloseDownloadRule() {
      this.downloadRuleForm = [];
      this.$refs["downloadRuleForm"].resetFields();
    },

    handleDelete(data) {
      var t = this;
      t.loading = true;
<<<<<<< HEAD
      var postUrl = "/wids/waf_del_wids_web_rule_check";
=======
      var postUrl = "/sys/waf_del_wids_web_rule_check";
>>>>>>> 32b45c66599ebbe10939a1aaf64e015bf985a11f
      var oData = {
        rule_name: data.rule_name
      };
      
      JXAjax(
        "post",
        postUrl,
        oData,
        function (response) {
          data.isVisiblePopover = false;
          t.loading = false;
          t.getData();
        },
        function () {
          t.loading = false;
        }
      );
    },
    onClickChangeOrder() { 
      var t = this;
      t.isShowOrder = !t.isShowOrder;
    },
    onClickChangeOrderSubmit(index, value, operate) { 
      var t = this;
      
      var oData = {
        rule_name: value.rule_name,
      };
      if (index > 0) { 
        if (operate == "top") {
          oData.type = "top";
        }
        if (operate == "up") {
          oData.type = "exchange";
          oData.exchange_name = t.tableData[index - 1].rule_name;
        }
      }

      if (index < t.tableData.length - 1) { 
        if (operate == "down") {
          oData.type = "exchange";
          oData.exchange_name = t.tableData[index + 1].rule_name;
        }
      }
<<<<<<< HEAD
      var postUrl = "/wids/waf_exchange_wids_web_rule_check_priority";
=======
      var postUrl = "/sys/waf_exchange_wids_web_rule_check_priority";
>>>>>>> 32b45c66599ebbe10939a1aaf64e015bf985a11f

      
      if (oData.type == "top" || oData.type == "exchange") { 
        t.orderLoading = true;  
        JXAjax(
          "post",
          postUrl,
          oData,
          function (response) {
            t.orderLoading = false;
            t.getData();
          },
          function () {
            t.orderLoading = false;
          },
          "no-message"
        );
      }
      
    },
    onChangeRuleStatus(value) { 
      var t = this;
      var oData = {
        rule_name: value.rule_name,
        status: value.status
      }
<<<<<<< HEAD
      var postUrl = "/wids/waf_edit_wids_web_rule_check_status";
=======
      var postUrl = "/sys/waf_edit_wids_web_rule_check_status";
>>>>>>> 32b45c66599ebbe10939a1aaf64e015bf985a11f
      JXAjax(
        "post",
        postUrl,
        oData,
        function (response) {
          t.getData();
        },
        function () {
          
        },
        "no-message"
      );
    }
  },
};
</script>
<style>


.custom-wrap {
  /* max-width: 800px;
  min-width: 400px; */
}
.el-button.button-block {
  display: block;
  margin-left: 0px;
  text-align: right;
  width: 100%;
}

.custom-wrap .rule-matchs-content .match-box-title {
  display: inline-block;
  text-align: left;
  font-size: 14px;
  color: #909399;
  padding: 0 5px;
  box-sizing: border-box;
}
.custom-wrap .rule-matchs-content .match-box-title:before {
  content: "*";
  color: #f56c6c;
  margin-right: 4px;
}
.custom-wrap .match-inline-block {
  width: 192px;
}
.custom-wrap .match-inline-block-small {
  width: 150px;
}
.box-card-rule {
  margin-bottom: 22px;
  border: 1px solid #409eff;
}
.box-card-rule .card-item {
  border-bottom: 1px solid #e0e3e9;
  margin-bottom: 20px;
}
</style>
