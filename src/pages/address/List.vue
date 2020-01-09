<template>
    <div>
        <el-button size="small" type="success" @click="toAddHandler">添加</el-button>
        <el-table :data="addresses">
            <el-table-column  prop="id" fixed="left" label="编号"></el-table-column>
            <el-table-column  prop="province" width="200" label="省"></el-table-column>
            <el-table-column  prop="city" width="200" label="市"></el-table-column>
            <el-table-column  prop="area" width="200" label="区"></el-table-column>
            <el-table-column  prop="address" width="200" label="地址"></el-table-column>
            <el-table-column  prop="telephone" width="200" label="电话"></el-table-column>
            <el-table-column  prop="customerId" width="200" label="顾客id"></el-table-column>
            <el-table-column  fixed="right" label="操作">
                <template v-slot="slot">
                    <a href="" @click.prevent="toUpdataHandler(slot.row)">修改</a>
                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                </template>
            </el-table-column>
        </el-table>
    <!-- 模态框 -->
        <el-dialog
            title="录入地址信息"
            :visible.sync="visible"
            width="60%">
            <el-form ref="form" :model="form" label-width="80px">
                <el-form-item label="省">
                    <el-input v-model="form.province"></el-input>
                </el-form-item>
                <el-form-item label="市">
                    <el-input v-model="form.city"></el-input>
                </el-form-item>
                 <el-form-item label="区">
                    <el-input v-model="form.area"></el-input>
                </el-form-item>
                <el-form-item label="地址">
                   <el-input v-model="form.address"></el-input>
                </el-form-item>
                <el-form-item label="电话">
                    <el-input v-model="form.telephone"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
            <el-button @click="closeModelHandler" size="small">取 消</el-button>
            <el-button type="primary" @click="submitHandler" size="small">确 定</el-button>
            </span>
        </el-dialog>

    <!-- 模态框 -->
    </div>
    
</template>
<script>
import querystring, { stringify } from 'querystring'
import request from '@/utils/request'
import { types } from 'util'
export default {
    //用于存放要向网页中存放的数据
    data(){
        return{
            visible:false,
            addresses:[],
            form:{
                type:"address"
            }
        }
    },
    methods:{
        loadData(){
            let url = "http://localhost:6677/address/findAll"
            request.get(url).then((response)=>{
            // 将查询结果设置到customers中，this指向外部函数的this
            //箭头函数的this指向外部函数的this
                this.addresses = response.data;
            })
        },
        toDeleteHandler(id) {
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
            }).then(() => {
                //alert(id);
                let url="http://localhost:6677/address/deleteById?id="+id;
                request.get(url).then((response)=>{
                    this.loadData();
                    this.$message({
                        type: 'success',
                        message: response.message
                    });  
                })        
            });
        },
        toAddHandler(){
            this.visible=true;
             this.form={
                 types:"address"
            };
        },
        closeModelHandler(){
            
            this.visible=false;
        },
        toUpdataHandler(row){
             this.visible=true;
            //模态框表单显示当前行的详细
             this.form=row;
        },
        submitHandler(){
            let url = "http://localhost:6677/address/saveOrUpdate";
            //前端向后台发送请求，完成数据的保存
            request({
                url,
                method:"POST",
                //告诉后台即将发送字符串，查询字符串
                headers:{
                "Content-Type":"application/x-www-form-urlencoded"
                },
                //将this.form转换为查询字符串转发给后台
                data:querystring.stringify(this.form)
            }).then((response)=>{
                // 模态框关闭
                this.closeModelHandler();
                // 刷新
                this.loadData();
                // 提示消息
                this.$message({
                    type:"success",
                    message:response.message
                })
            })
        } 
    },
    created(){
        this.loadData();
    }
}
</script>
<style scoped>
</style>