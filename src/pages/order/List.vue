<template>
  <div>
    <el-tabs v-model="activeName" @tab-click="handleClick">
    <el-tab-pane label="所有订单" name="first">
      <el-table :data="orders.list">
        <el-table-column prop="id" label="订单编号"></el-table-column>
        <el-table-column prop="orderTime" label="下单时间"></el-table-column>
        <el-table-column prop="total" label="总价"></el-table-column>
        <el-table-column prop="status" label="状态"></el-table-column>
        <el-table-column prop="customerId" label="顾客ID"></el-table-column>
        <el-table-column prop="waiterId" label="员工ID"></el-table-column>
        <el-table-column prop="addressId" label="地址ID"></el-table-column>
        <el-table-column label="操作">
          <template v-slot="slot">
            <a href="" @click.prevent="">详情</a>
          </template>
        </el-table-column>
    </el-table>
    </el-tab-pane>
    <el-tab-pane label="待支付" name="second">
      <el-table :data="orders.list">
        <el-table-column prop="id" label="订单编号"></el-table-column>
        <el-table-column prop="orderTime" label="下单时间"></el-table-column>
        <el-table-column prop="total" label="总价"></el-table-column>
        <el-table-column prop="status" label="状态"></el-table-column>
        <el-table-column prop="customerId" label="顾客id"></el-table-column>
      </el-table>
    </el-tab-pane>
    <el-tab-pane label="待派单" name="third">
      <el-table :data="orders.list">
        <el-table-column prop="id" label="订单编号"></el-table-column>
        <el-table-column prop="orderTime" label="下单时间"></el-table-column>
        <el-table-column prop="total" label="总价"></el-table-column>
        <el-table-column prop="status" label="状态"></el-table-column>
        <el-table-column prop="customerId" label="顾客id"></el-table-column>
        <el-table-column label="操作">
          <template v-slot="slot">
            <a href="" @click.prevent="sendOrder(slot.row.id)">派单</a>
          </template>
        </el-table-column>
      </el-table>
    </el-tab-pane>
    <el-tab-pane label="待接单" name="fourth">
      <el-table :data="orders.list">
        <el-table-column prop="id" label="订单编号"></el-table-column>
        <el-table-column prop="orderTime" label="下单时间"></el-table-column>
        <el-table-column prop="total" label="总价"></el-table-column>
        <el-table-column prop="status" label="状态"></el-table-column>
        <el-table-column prop="customerId" label="顾客id"></el-table-column>
        <el-table-column label="操作"> 
        </el-table-column>
      </el-table>
    </el-tab-pane>
    <el-tab-pane label="待服务" name="fifth">
      <el-table :data="orders.list">
        <el-table-column prop="id" label="订单编号"></el-table-column>
        <el-table-column prop="orderTime" label="下单时间"></el-table-column>
        <el-table-column prop="total" label="总价"></el-table-column>
        <el-table-column prop="status" label="状态"></el-table-column>
        <el-table-column prop="customerId" label="顾客id"></el-table-column>
        <el-table-column prop="waiterId" label="员工id"></el-table-column>
      </el-table>
    </el-tab-pane>
    <el-tab-pane label="待确认" name="sixth">
      <el-table :data="orders.list">
        <el-table-column prop="id" label="订单编号"></el-table-column>
        <el-table-column prop="orderTime" label="下单时间"></el-table-column>
        <el-table-column prop="total" label="总价"></el-table-column>
        <el-table-column prop="status" label="状态"></el-table-column>
        <el-table-column prop="customerId" label="顾客id"></el-table-column>
        <el-table-column prop="waiterId" label="员工id"></el-table-column>
        </el-table>
    </el-tab-pane>
    <el-tab-pane label="已完成" name="seventh">
      <el-table :data="orders.list">
        <el-table-column prop="id" label="订单编号"></el-table-column>
        <el-table-column prop="orderTime" label="下单时间"></el-table-column>
        <el-table-column prop="total" label="总价"></el-table-column>
        <el-table-column prop="status" label="状态"></el-table-column>
        <el-table-column prop="customerId" label="顾客id"></el-table-column>
        <el-table-column prop="waiterId" label="员工id"></el-table-column>
      </el-table>
    </el-tab-pane>
  </el-tabs>
    <!--分页开始 -->
       <el-pagination 
        layout="prev, pager, next" 
        :total="orders.total" 
        @current-change="pageChageHandler">
        </el-pagination>
    <!--/分页结束 -->
    <!-- 模态框 -->
    <el-dialog
            title="派单"
            :visible.sync="visible"
            width="60%">
            <template>
                <el-radio-group v-model="radio">
                    <el-radio :label="1">员工1</el-radio>
                    <el-radio :label="2">员工2</el-radio>
                    <el-radio :label="3">员工3</el-radio>
                </el-radio-group>
              </template>
            <span slot="footer" class="dialog-footer">
            <el-button @click="visible=false" size="small">取 消</el-button>
            <el-button type="primary" @click="visible = false" size="small">确 定</el-button>
            </span>
      </el-dialog>
    <!-- /模态框 -->

  </div>
</template>
<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
  // 用于存放网页中需要调用的方法
  methods:{
    sendOrder(){
      this.visible = true;
    },
    //当分页中当前页改变时候执行
    pageChageHandler(page){
        //将params中的当前页改为插件中的当前页
        this.params.page = page-1;
        //加载
        this.loadData();
    },
    loadData(){
      let url = "http://localhost:6677/order/queryPage"
    //   request.get(url).then((response)=>{
    //     // 将查询结果设置到customers中，this指向外部函数的this
    //     this.allorder = response.data;
    //   })
    request({
      url,
      method:"post",
      headers:{
        "Content-Type":"application/x-www-form-urlencoded"
      },
      data:querystring.stringify(this.params)
    }).then((response)=>{
      //orders为一个对象，其中包含了分页信息，以及列表信息
      this.orders = response.data;
    })
    },
    submitHandler(){
      //this.form 对象 ---字符串--> 后台 {type:'customer',age:12}
      // json字符串 '{"type":"customer","age":12}'
      // request.post(url,this.form)
      // 查询字符串 type=customer&age=12
      // 通过request与后台进行交互，并且要携带参数
      let url = "http://localhost:6677/order/saveOrUpdate";
      request({
        url,
        method:"POST",
        headers:{
          "Content-Type":"application/x-www-form-urlencoded"
        },
        data:querystring.stringify(this.form)
      }).then((response)=>{
        // 模态框关闭
        this.closeModalHandler();
        // 刷新
        this.loadData();
        // 提示消息
        this.$message({
          type:"success",
          message:response.message
        })
      })
    },
    toDeleteHandler(id){
      this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        //调用后台接口，完成删除操作
        let url = "http://localhost:6677/order/deleteById?id"+id;
        request.get(url).then((response)=>{
          //刷新数据
          this.loadData();
          //提示结果
          this.$message({
          type: 'success',
          message: response.message
        });
        })
      })
    },
    toUpdateHandler(row){
      //模态框表单中显示当前行信息
      this.form = row;
      this.visible = true;
    },
    closeModalHandler(){
      this.visible = false;
    },
    toAddHandler(){
      this.foem = {
        type : "order"
      }
      this.visible = true;
    }
  },
  // 用于存放要向网页中显示的数据
  data(){
    return {
      radio:'1',
      radio:'2',
      radio:'3',
      visible:false,
      orders:{},
      
      form:{
        type:"orders"
      },
      
      params:{
        page:0,
        pageSize:10
      }
    }
  },
  created(){
    // this为当前vue实例对象
    // vue实例创建完毕 
    this.loadData()

  }
} 
</script>

<style scoped>
 
</style>