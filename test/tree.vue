<template>
    <div class="orderlist">
        <section id="list-order">
            <div class="order-header">
                <div class="list-title fl">
                    <div class="ts"></div>
                    <span class="tilte">订单列表</span>
                </div>
                <!-- <div class="refresh fr">
                    <i class="fl"></i>
                    <span class="refname fr">刷新</span>
                </div> -->
            </div>
            <!-- 筛选查询 -->
            <!-- <div class="screen">
                 <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
                <div class="screen-header">
                    <div class="screen-title fl">
                        <i class="fl"></i>
                        <span class="fr">筛选查询</span>
                    </div>
                    <div class="btn fr">
                       
                         <el-form-item>
                              <el-button @click="submitForm('ruleForm')" class="fl">查询</el-button>
                              <el-button @click="resetForm('ruleForm')" class="fr">重置</el-button>
                          </el-form-item>
                    </div>
                </div>
                <div class="condition" style='display:flex;clear:both;padding-top: 14px;'>
                    <el-form-item label="订单号：" prop="size" :rules="rulesCusto()" >
                             <el-input v-model="ruleForm.size" placeholder="订单编号"></el-input>
                    </el-form-item>
                    <el-form-item label="订单状态:" prop="sizeCheck">
                          <el-select v-model="ruleForm.sizeCheck" placeholder="请选择">
                            <el-option
                            v-for="item in options"
                            :key="item.value"
                            :label="item.label"
                            :value="item.id">
                            </el-option>
                        </el-select>
                    </el-form-item>
                     <el-form-item label="下单时间:" prop="startTime">
                          <el-time-select
                            placeholder="起始时间"
                            v-model="ruleForm.startTime"
                            :picker-options="{
                            start: '08:30',
                            step: '00:15',
                            end: '18:30'
                            }">
                        </el-time-select>
                        
                    </el-form-item>
                    
                   
                         <el-form-item  prop="endTime">
                          <span style='margin:0 10px;'>至</span>
                      
                        <el-time-select
                            placeholder="结束时间"
                            v-model="ruleForm.endTime"
                            :picker-options="{
                            start: '08:30',
                            step: '00:15',
                            end: '18:30',
                            minTime:ruleForm.startTime
                            }">
                        </el-time-select>
                    </el-form-item>
                     <el-form-item label="会员账号:" prop="account">
                            <el-input v-model="ruleForm.account" placeholder="手机号码"></el-input>
                    </el-form-item>
                    
                    
                </div>
                 </el-form>
            </div> -->
            <!-- 筛选查询 -->
            <div class="screen">
                <div class="screen-header">
                    <div class="screen-title fl">
                        <i class="fl"></i>
                        <span class="fr">筛选查询</span>
                    </div>
                    <div class="btn fr">
                        <span class="fl"  @click="submitForm(1)">查询</span>
                        <span class="fr"  @click="submitForm(2)">重置</span>
                    </div>
                </div>
                <div class="condition">
                    <div class="sr-search fl">
                        <span class="order-dh">订单号:</span>
                        <el-input v-model="ruleForm.size" placeholder="订单编号"></el-input>
                    </div>
                    <div class="order-status fl">
                        <span>订单状态:</span>
                        <el-select  v-model="ruleForm.sizeCheck" placeholder="请选择">
                            <el-option
                            v-for="item in options"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>
                    </div>
                    <div class="sr-search fl">
                        <span>下单时间:</span>
                        <el-time-select
                            placeholder="起始时间"
                           v-model="ruleForm.startTime"
                            :picker-options="{
                            start: '08:30',
                            step: '00:15',
                            end: '18:30'
                            }">
                        </el-time-select>
                        <span class="sec">至</span>
                        <el-time-select
                            placeholder="结束时间"
                             v-model="ruleForm.endTime"
                            :picker-options="{
                            start: '08:30',
                            step: '00:15',
                            end: '18:30',
                            minTime: startTime
                            }">
                        </el-time-select>
                    </div>
                    <div class="order-status fl">
                        <span>会员账号:</span>
                        <el-input v-model="ruleForm.account" placeholder="订单编号"></el-input>
                    </div>
                </div>
            </div>

            <!-- 数据列表 -->
            <div class="list-content">
                <div class="list-title">
                    <i class="fl"></i>
                    <span class="fl">订单列表</span>
                </div>
                <div class="order-num">
                    <div class="num-menu">
                        <span class="menu-sp">商品</span>
                        <span class="menu-dd">单价</span>
                        <span class="menu-sl">数量</span>
                        <span class="menu-hy">会员账号</span>
                        <span class="menu-yh">优惠信息</span>
                        <span class="menu-je">订单金额</span>
                        <span class="menu-sf">实付款</span>
                        <span class="menu-zt">订单状态</span>
                        <span class="menu-cz">操作</span>
                    </div>
                    <!-- 订单信息 -->
                      <div v-for='(item,index) in datas' :key='index'>
                        <div class="num-bh">
                              <input type="checkbox" class='selBtn' @click='shoperCheck(index)'>
                            <span>{{item.createTime}}</span>
                            <span>订单号:{{item.orderNo}}</span>
                        </div>
                        <div class="num-infor" v-for='(demo,idx) in item.goodsList' :key='idx' :id='item.goodsId'>
                            <div class="menu-sp ks-clear order-list-sp">
                                <i class="fl">
                                  <img :src="demo.skuImg" alt="">
                                </i>
                                <div class="order-list-name fl"> 
                                    <span>商品名称:{{demo.title}}</span>
                                    <br>
                                    <span>(规格信息){{demo.skuName}}</span>
                                    <!-- <span>尺寸:x1</span> -->
                                </div>
                            </div>
                            <div class="menu-dd order-list-dd">
                                <span>¥{{demo.price}}</span>
                            </div>
                            <div class="menu-sl order-list-dd">
                                <span>{{demo.num}}</span>
                            </div>
                            <div class="menu-hy  order-list-dd">
                                <span>{{item.member}}</span>
                            </div>
                            <div class="menu-yh  order-list-yh">
                                <span>平台优惠卷:¥{{item.redDeduction}}</span>
                                <span>商家优惠卷:¥{{item.sellerRedDeduction}}</span>
                            </div>
                            <div class="menu-je  order-list-dd">
                                <span>¥{{item.totalAmt}}</span>
                            </div>
                            <div class="menu-sf order-list-sf">
                                <span>¥{{item.payAmount}}</span>
                                <span>(含运费:¥{{item.freight}})</span> 
                                <span class="order-red-bor apporder">APP订单</span>
                            </div>
                            <div class="menu-zt  order-list-dd">
                                <span v-if='item.status ==1'> 待支付</span>
                                <span v-else-if='item.status ==2'> 代发货</span>
                                <span v-else-if='item.status ==3'> 待收货</span>
                                <span v-else> 已完成</span>
                            </div>
                            <div class="menu-cz order-list-cz">
                                <span class="order-red-bor">查看订单</span>
                                <span class="order-red-bor">物流跟踪</span>
                            </div>
                        </div>
                    </div>

                </div>                
            </div>           
            <!-- 底部 -->
            <div class="foot" v-show='ageCheck'>
              <div style='float:left;'>
                <input type="checkbox" v-model='allBtn'  @click='allChecked($event)'>
                <span class="mright">全选</span>
                <span class="foot-sty" @click='deleshopList'>删除</span>
                </div>
                <div class="block">
                       <PageIng :total='page.total' @pageChange='pageChange' v-show='pageCheck'></PageIng>
                </div>
            </div>
        </section>
    </div>
</template>

<script>
import PageIng from "@/frame/PagIng";
export default {
  data() {
    return {
      allBtn: false,
      shoperIndex: "", //商品下标
      datas: "",
      pageCheck: true,
      options: [
        {
          value: "选项1",
          label: "待付款",
          id: "1"
        },
        {
          value: "选项2",
          label: "待发货",
          id: "2"
        },
        {
          value: "选项3",
          label: "待收货",
          id: "3"
        },
        {
          value: "选项4",
          label: "已完成",
          id: "4"
        },
        {
          value: "选项5",
          label: "已关闭",
          id: "5"
        },
        {
          value: "选项6",
          label: "已删除",
          id: "6"
        },
        {
          value: "选项7",
          label: "退款退货中",
          id: "7"
        }
      ],
      num: [
        {
          value: "选项1",
          label: "20"
        },
        {
          value: "选项2",
          label: "50"
        },
        {
          value: "选项3",
          label: "100"
        },
        {
          value: "选项4",
          label: "200"
        }
      ],
      del: [
        {
          value: "选项1",
          label: "删除"
        }
      ],
      delvalue: "",
      member: "", //会员账号
      checked: false,
      ruleForm: {
        size: "",
        sizeCheck: "",
        account: "",
        startTime: "",
        endTime: ""
      },
      rules: {
        size: [
          { required: true, message: "请输入订单号", trigger: "blur" }
          //   { min: 3, max: 5, message: "长度在 3 到 5 个字符", trigger: "blur" }
        ],
        sizeCheck: [
          { required: true, message: "请选择订单状态", trigger: "change" }
        ],
        startTime: [
          { required: true, message: "请选择起始时间", trigger: "change" }
        ],
        endTime: [
          { required: true, message: "请选择结束时间", trigger: "change" }
        ],
        account: [
          { required: true, message: "请输入会员账号", trigger: "blur" }
        ]
      },
      page: {
        pageNo: 1, //页数
        pageSize: 5, //条数
        total: ""
      }
    };
  },
  created() {
    this.commonAjax();
    if (this.datas.length < 0) {
      this.$message.info("这里空空的，还没有相关的订单信息.");
      this.pageCheck = false;
      return;
    }
  },
  methods: {
    // 分页器
    pageChange(item) {
      this.page.pageNo = item.page;
      this.page.pageSize = item.limit;
      console.log(item);
    },
    commonAjax() {
      this.$ajx(
        this.$apiHead("order") + "/sellerOrder/list",
        {
          status: this.ruleForm.sizeCheck,
          pageNo: this.page.pageNo,
          pageSize: this.page.pageSize,
          orderNo: this.ruleForm.size,
          mobile: this.ruleForm.account,
          startTime: this.ruleForm.startTime,
          endTime: this.ruleForm.endTime
        },
        data => {
          if (data.code != 0) {
            this.$message.info(data.info);
            return;
          }
          this.datas = data.info;
          this.page.total = data.info.length;
        },
        err => {
          console.log(err);
        }
      );
    },
    // 查询按钮
    submitForm(idx) {
      if (idx == 2) {
        this.ruleForm;
        for (var key in this.ruleForm) {
          this.ruleForm[key] = "";
        }
      }
      if (idx == 1) {
        if (this.datas.length > 0) {
          // alert("提交submit");
          this.commonAjax();
        } else {
          this.$message.info("这里空空的，还没有相关的订单信息.");
          return;
        }
      }
    },
    // 重置按钮
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
    },
    // 全选
    allChecked(e) {
      let checked = e.target.checked;
      // this.checkboxCase = !this.checkboxCase;
      let checkDom = document.getElementsByClassName("selBtn");
      let idArray = [];
      if (checked) {
        for (var i = 0; i < checkDom.length; i++) {
          checkDom[i].checked = true;
        }
      } else {
        for (var i = 0; i < checkDom.length; i++) {
          checkDom[i].checked = false;
        }
      }
    },
    // 商品选择项  多选按钮
    shoperCheck(idx) {
      this.shoperIndex = idx;
      let checkDom = document.getElementsByClassName("selBtn");
      for (var i = 0; i < checkDom.length; i++) {
        if (!checkDom[i].checked) {
          this.allBtn = false;
          return;
        }
      }
      this.allBtn = true;
    },
    // 删除数据列表
    deleshopList() {
      alert(this.shoperIndex);
      let checkDom = document.getElementsByClassName("selBtn");
      for (var i = 0; i < checkDom.length; i++) {
        if (checkDom[i].checked) {
          this.datas.splice(this.shoperIndex, 1);
          alert(this.shoperIndex + "下架商品下标");
          return false;
        }
      }
      this.$message.warning({ message: "请选择要删除的商品哦！" });
    }
  },
  components: {
    PageIng
  }
};
</script>

<style lang="scss">
@import "../style/less/_base.scss";
.orderlist {
  .el-form-item__content {
    margin-left: 0 !important;
    display: flex;
  }
  .el-form-item.is-required:not(.is-no-asterisk) > .el-form-item__label:before {
    color: transparent;
  }
  .el-input__inner {
    border-radius: 0px;
  }

  .el-button {
    padding: 9px 20px;
  }
  input {
    background: none;
  }
  .fl {
    float: left;
  }
  .fr {
    float: right;
  }
  .cleafix:after {
    content: "";
    display: block;
    clear: both;
  }
  #list-order {
    width: 1200px;
    height: 100%;
    margin: 0 auto;
    font-family: "Microsoft YaHei";
    font-size: 16px;
  }
  .order-header {
    height: 58px;
    .list-title {
      margin-top: 20px;
      .ts {
        width: 6px;
        height: 20px;
        background-color: #e62828;
        display: inline-block;
        margin-right: 13px;
      }
      .tilte {
        font-size: 20px;
        color: #333333;
        font-weight: bold;
      }
    }
    .refresh {
      width: 80px;
      height: 32px;
      background: rgba(255, 255, 255, 1);
      border: 1px solid rgba(224, 224, 224, 1);
      text-align: center;
      margin: 14px 20px 0 0;
      i {
        width: 20px;
        height: 20px;
        background-image: url(../../static/images/sx.png);
        background-repeat: no-repeat;
        margin: 5px 8px 5px 9px;
      }
      .refname {
        margin: 5px 8px 0 0;
      }
    }
  }

  /* 筛选查询 */
  .screen {
    height: 154px;
    background: rgba(255, 255, 255, 1);
    border: 1px solid rgba(224, 224, 224, 1);
    margin-bottom: 30px;
  }

  .screen-header {
    height: 48px;
    background: rgba(245, 245, 245, 1);
    border-top: 1px solid rgba(224, 224, 224, 1);
    border-bottom: 1px solid rgba(224, 224, 224, 1);
    margin-top: 30px;
    .screen-title {
      width: 94px;
      height: 32px;
      margin-left: 18px;
      padding-top: 16px;
      i {
        width: 16px;
        height: 16px;
        background-image: url(../../static/images/search.png);
        background-repeat: no-repeat;
      }
    }
    .btn {
      // width: 198px;
      height: 40px;
      margin-right: 19px;
      padding-top: 8px;
      // span {
      //   width: 90px;
      //   height: 25px;
      //   background: rgba(255, 255, 255, 1);
      //   border: 1px solid rgba(224, 224, 224, 1);
      //   text-align: center;
      //   padding-top: 7px;
      // }
    }
  }
  .screen {
    height: 154px;
    background: rgba(255, 255, 255, 1);
    border: 1px solid rgba(224, 224, 224, 1);
    margin-bottom: 30px;
    .screen-header {
      height: 48px;
      background: rgba(245, 245, 245, 1);
      border-top: 1px solid rgba(224, 224, 224, 1);
      border-bottom: 1px solid rgba(224, 224, 224, 1);
      margin-top: 30px;
      .screen-title {
        width: 94px;
        height: 32px;
        margin-left: 18px;
        padding-top: 16px;
        i {
          width: 16px;
          height: 16px;
          background-image: url(../../static/images/search.png);
          background-repeat: no-repeat;
        }
      }
      .btn {
        width: 198px;
        height: 40px;
        margin-right: 19px;
        padding-top: 8px;
        span {
          width: 90px;
          height: 25px;
          background: rgba(255, 255, 255, 1);
          border: 1px solid rgba(224, 224, 224, 1);
          text-align: center;
          padding-top: 7px;
        }
      }
    }
  }

  .condition {
    height: 76px;
    line-height: 76px;
    padding-right: 10px;
    .sr-search {
      margin-right: 20px;
      .el-input {
        width: 160px;
      }
      .el-input__inner {
        width: 160px;
        height: 34px;
        border-radius: 0px;
        display: inline-block;
      }
      .order-dh {
        margin-left: 23px;
      }
      .sec {
        margin: 0 10px;
      }
    }
    .order-status {
      margin-right: 20px;
      .el-input {
        width: 120px;
      }
      .el-input__inner {
        width: 120px;
        height: 34px;
        border-radius: 0px;
        display: inline-block;
      }
    }
  }

  /* 数据列表 */
  .list-content {
    width: 1200px;
    .list-title {
      height: 50px;
      border: 1px solid #e0e0e0;
      background-color: #fff;
      i {
        width: 23px;
        height: 16px;
        background-image: url(../../static/images/list.png);
        background-repeat: no-repeat;
        background-size: 23px 16px;
        margin: 15px;
      }
      span {
        line-height: 48px;
      }
    }
    .order-num {
      .num-menu {
        height: 50px;
        line-height: 50px;
        background-color: #fff;
        font-weight: bold;
        text-align: center;
        span {
          float: left;
          margin-right: 20px;
        }
      }
      .menu-sp {
        width: 319px;
      }
      .menu-dd {
        width: 94px;
      }
      .menu-sl {
        width: 65px;
      }
      .menu-hy {
        width: 90px;
      }
      .menu-yh {
        width: 120px;
      }
      .menu-je {
        width: 88px;
      }
      .menu-sf {
        width: 102px;
      }
      .menu-zt {
        width: 72px;
      }
      .menu-cz {
        width: 70px;
      }
      .order-red-bor {
        width: 66px;
        height: 22px;
        border: 1px solid #e62828;
        border-radius: 4px;
        line-height: 22px;
        text-align: center;
        color: #e62828;
      }
      .num-bh {
        height: 50px;
        line-height: 50px;
        .el-checkbox__inner {
          margin-left: 20px;
        }
      }
      .num-infor {
        height: 100px;
        font-size: 12px;
        background: #fff;
        div {
          float: left;
          margin-right: 20px;
        }
        .order-list-sp {
          i {
            width: 70px;
            height: 70px;
            background-color: #f6f6f6;
            margin: 15px;
          }
          .order-list-name {
            line-height: 18px;
            width: 191px;
            div {
              margin: 15px 0 24px 0;
            }
          }
        }
        .order-list-dd {
          line-height: 100px;
          text-align: center;
        }
        .order-list-yh {
          margin-top: 20px;
          span {
            display: block;
            line-height: 31px;
          }
        }
        .order-list-sf {
          margin-top: 15px;
          span {
            display: block;
            text-align: center;
            line-height: 20px;
          }
          .apporder {
            margin: 5px 0 0 20px;
          }
        }
        .order-list-cz span {
          display: block;
          margin-top: 20px;
        }
      }
    }
  }

  /* 底部 */
  .foot {
    height: 42px;
    margin-top: 40px;
    padding-top: 10px;
    .foot-sty {
      display: inline-block;
      width: 90px;
      height: 27px;
      background: rgba(255, 255, 255, 1);
      border: 1px solid rgba(224, 224, 224, 1);
      text-align: center;
      padding-top: 5px;
    }
    .el-input__inner {
      width: 110px;
      height: 32px;
      border-radius: 0;
      background-color: #fff;
    }
    .el-input__icon {
      line-height: 34px;
    }

    .block {
      display: inline-block;
      float: right;
    }
    .mright {
      margin-right: 10px;
    }
    .el-pagination__editor.el-input .el-input__inner {
      width: 34px;
    }
  }
}
</style>
