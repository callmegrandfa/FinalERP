<template>
        <!-- 基础资料-短开始 -->
        <div class="short-basic-data container-fluid">
            <!-- 搜索框部分开始 -->
            <el-row class="search-component">
                <el-col :span='2' class="add-btn green-btn">
                    <span>新增</span>
                </el-col>

                <el-col :span='2' class="import-btn green-btn">
                    <span>导入</span>
                </el-col>

                <el-col :span='5' class="search-input">
                    <input type="text" class="form-control" placeholder="编码/名称..."/>
                </el-col>

                <el-col :span='2' class="search-btn" >
                    <span>搜索</span>
                </el-col>

                <el-col :span='2' class="high-search">
                    <span @click='searchShow'>高级搜索</span>
                </el-col>

                <el-col :span='2' class="gray-btn">
                    <span>导出</span>
                </el-col>
            
                <el-col :span='2' class="gray-btn">
                    <span>打印</span>
                </el-col>
            </el-row>
            <!-- 搜索框部分结束 -->

            <!-- 基础资料表单开始 -->
            <el-row class="basic-table">
                <el-col :span='4' class="tree-container">
                    <el-tree :data="componyTree" :props="defaultProps" @node-click="handleNodeClick"></el-tree>
                </el-col>

                <el-col :span='20'>
                    <el-table :data="tableData" border style="width: 100%" stripe>
                        <el-table-column prop="sequence" label="序号" ></el-table-column>
                        <el-table-column prop="planCode" label="班次方案代码" ></el-table-column>
                        <el-table-column prop="planName" label="班次方案名称"></el-table-column>
                        <el-table-column prop="remark" label="备注">
                            <template scope="scope">
                                <input  type="text" :disabled="scope.$index!=isEdit" v-on:blur="finishEdit(scope.$index)"/>
                            </template>
                        </el-table-column>
                        <el-table-column prop="ifAllow" label="允许使用">
                            <template scope="scope">
                                <el-checkbox v-model="tableData[scope.$index].ifAllow" ></el-checkbox>
                            </template>
                        </el-table-column>
                        <el-table-column prop="updateDate" label="修改时间"></el-table-column>
                        <el-table-column label="操作">
                            <template slot-scope="scope">
                                <el-button v-on:click="handleEdit(scope.$index)" type="text"  size="small">修改</el-button>
                                <!-- <el-button v-show='scope.$index==ifSave' v-on:click="handleSave(scope.$index)" type="text" size="small">保存</el-button>  -->
                                <el-button v-on:click="handleDelete(scope.$index)" type="text" size="small">删除</el-button>
                            </template>
                        </el-table-column>
                    </el-table> 
                    <el-pagination style="margin-top:20px;" class="text-right" background layout="total, prev, pager, next"  :page-count="totalPage" v-on:current-change="handleCurrentChange"></el-pagination>   
                </el-col>
            </el-row>
            <!-- 基础资料表单结束 -->

            <!-- 高級搜索開始 -->
            <div class="high-search-container" v-if='highSearchShow'>
                
                <el-row class="high-search-header">
                    <el-col :span='6'>
                        <span>高级搜索</span>
                    </el-col>
                    <el-col :span='6'>
                       <span class="super-search"> 超级搜索</span>
                    </el-col>
                    <el-col :span='3' :offset="8" style="padding-top:10px;">
                        <img src="../../assets/close.png" style="cursor:pointer;" @click="closeHighSearch">
                    </el-col>
                </el-row>

                <el-row>
                    <el-col :span='5' :offset="1" class="search-plan">
                        <span>查询方案</span>
                    </el-col>
                    <el-col :span='4' :offset='10' class="search-edit">
                        <span @click="doEdit" v-if="isChoose">编辑</span>
                        <span @click="noEdit" v-if='noChoose'>保存</span>
                    </el-col>
                    <el-col :span='4'>
                        <el-row>
                            <el-col :span='12'>
                                <span>全部</span>
                            </el-col>
                            <el-col :span='12'>
                                <img src="../../assets/down.png" alt="">
                            </el-col>
                        </el-row>
                    </el-col>
                </el-row>

                <ul class="this-week">
                    <li v-if='week1'>
                        <span>本周未审核</span>
                        <div @click='cancelweek1'>
                            <img class="white-one" 
                                src="../../assets\whiteone.png"
                                v-if='canEdit'>

                            <img class="red-bg" 
                                src="../../assets\redcircle.png" 
                                v-if='canEdit'>
                        </div>
                    </li>

                    <li v-if='week2'>
                        <span>本周已审核</span>
                        <div @click='cancelweek2'>
                            <img class="white-one" 
                                src="../../assets\whiteone.png"
                                v-if='canEdit'>

                            <img class="red-bg" 
                                src="../../assets\redcircle.png" 
                                v-if='canEdit'>
                        </div>
                    </li>
                </ul>

                <el-row>
                    <el-col :span='20' offset='1'>
                        <span>查询条件</span>
                    </el-col>
            
                    <el-col :span='2' class="search-add">
                        <span>添加</span>
                    </el-col>
                </el-row>

                <el-row class="search-num">
                    <el-col :span='18' offset='1' 
                            style="border:1px solid #cccccc;padding-left:10px;">
                        <input type="text" 
                                class="form-control" 
                                placeholder="请输入查询单号..." 
                                style="width:100%">
                    </el-col>
                </el-row>

                <el-row class="data-status">
                    <el-col :span='3' :offset='1' style="padding-top:5px">
                        <span>单据状态</span>
                    </el-col>
                    <el-col :span='18' :offset="2">
                        <ul  class="data-ul">
                            <li v-for='item in alreadyDo'>
                                <el-checkbox v-model="item.status" ></el-checkbox>
            
                                <div class="status-ready">
                                    <span>{{item.name}}</span>
                                </div>
                            </li>
                        </ul>
                    </el-col>
                </el-row>

                <el-row class="data-status">
                    <el-col :span='3' :offset='1' style="padding-top:5px">
                        <span>业务状态</span>
                    </el-col>

                    <el-col :span="18" :offset="2">
                         <ul  class="data-ul">
                            <li v-for='item in businessStatus'>
                                <el-checkbox v-model="item.status" ></el-checkbox>
            
                                <div class="status-ready">
                                    <span>{{item.name}}</span>
                                </div>
                            </li>
                        </ul>
                    </el-col>
                </el-row>

                <el-row class="search-depend">
                    <el-col :span="3" :offset="1">
                        <span>货号</span>
                    </el-col>
                    <el-col :span="15" :offset="2" class="search-depend-after">
                        <el-row class="search-box">
                            <el-col :span="2" class="logo-p">
                                 <img src="../../assets\searchLogo.png" alt="">
                            </el-col>
                           <el-col :span="18" :offset="1">
                               <input type="text" placeholder="请输入货号">
                           </el-col>
                        </el-row> 
                    </el-col>
                </el-row>

                <el-row class="search-depend">
                    <el-col :span="3" :offset="1">
                        <span>供应商</span>
                    </el-col>
                    <el-col :span="15" :offset="2" class="search-depend-after">
                        <el-row class="search-box">
                            <el-col :span="2" class="logo-p">
                                 <img src="../../assets\searchLogo.png" alt="">
                            </el-col>
                           <el-col :span="18" :offset="1">
                               <input type="text" placeholder="请输入供应商">
                           </el-col>
                        </el-row> 
                    </el-col>
                </el-row>

                <el-row class="search-depend">
                    <el-col :span="3" :offset="1">
                        <span>仓库</span>
                    </el-col>
                    <el-col :span="15" :offset="2" class="search-depend-after">
                        <el-row class="search-box">
                            <el-col :span="2" class="logo-p">
                                 <img src="../../assets\searchLogo.png" alt="">
                            </el-col>
                           <el-col :span="18" :offset="1">
                               <input type="text" placeholder="请输入仓库">
                           </el-col>
                        </el-row> 
                    </el-col>
                </el-row>

                 <el-row class="search-depend">
                    <el-col :span="3" :offset="1">
                        <span>PO单号</span>
                    </el-col>
                    <el-col :span="15" :offset="2" class="search-depend-after">
                        <el-row class="search-box">
                            <el-col :span="2" class="logo-p">
                                 <img src="../../assets\searchLogo.png" alt="">
                            </el-col>
                           <el-col :span="18" :offset="1">
                               <input type="text" placeholder="请输入PO单号">
                           </el-col>
                        </el-row> 
                    </el-col>
                </el-row>

                 <el-row class="search-depend">
                    <el-col :span="3" :offset="1">
                        <span>PO类型</span>
                    </el-col>
                    <el-col :span="15" :offset="2" class="search-depend-after">
                        <el-row class="search-box">
                            <el-col :span="2" class="logo-p">
                                 <img src="../../assets\searchLogo.png" alt="">
                            </el-col>
                           <el-col :span="18" :offset="1">
                               <input type="text" placeholder="请输入PO类型">
                           </el-col>
                        </el-row> 
                    </el-col>
                </el-row>

                 <el-row class="search-depend">
                    <el-col :span="3" :offset="1">
                        <span>品牌</span>
                    </el-col>
                    <el-col :span="15" :offset="2" class="search-depend-after">
                        <el-row class="search-box">
                            <el-col :span="2" class="logo-p">
                                 <img src="../../assets\searchLogo.png" alt="">
                            </el-col>
                           <el-col :span="18" :offset="1">
                               <input type="text" placeholder="请输入品牌">
                           </el-col>
                        </el-row> 
                    </el-col>
                </el-row>
                
                <el-row class="search-date">
                    <el-col :span="3" :offset="1">
                        <span>开单日期</span>
                    </el-col>
                    <el-col :span="15" :offset="2" class="search-date-after">
                        <el-date-picker
                            v-model="billingDate"
                            type="daterange"
                            start-placeholder="开始日期"
                            end-placeholder="结束日期"
                            default-value="2010-10-01">
                            </el-date-picker>
                    </el-col>
                </el-row>

                <el-row class="search-date">
                    <el-col :span="3" :offset="1">
                        <span>生效日期</span>
                    </el-col>
                    <el-col :span="15" :offset="2" class="search-date-after">
                        <el-date-picker
                            v-model="workDate"
                            type="daterange"
                            start-placeholder="开始日期"
                            end-placeholder="结束日期"
                            default-value="2010-10-01">
                            </el-date-picker>
                    </el-col>
                </el-row>

                <el-row class="search-date">
                    <el-col :span="3" :offset="1">
                        <span>交货日期</span>
                    </el-col>
                    <el-col :span="15" :offset="2" class="search-date-after">
                        <el-date-picker
                            v-model="dealDate"
                            type="daterange"
                            start-placeholder="开始日期"
                            end-placeholder="结束日期"
                            default-value="2010-10-01">
                            </el-date-picker>
                    </el-col>
                </el-row>

                <el-row  class="last-btn">
                    <el-col :span="3" :offset="3">
                        <span class="s-btn">查询</span>
                    </el-col>
                    <el-col :span="3" :offset="3">
                        <span class="r-btn">重置</span>
                    </el-col>
                    <el-col :span="3" :offset="3">
                        <span class="c-btn">取消</span>
                    </el-col>
                </el-row>
            </div>
            <!-- 高級搜索結束 -->
        </div>
        <!-- 基础资料短-结束 -->
</template>

<script>

export default {
  name: 'shortdata',
  data(){
      return{
            highSearchShow:false,//控制高级搜索显示
			canEdit:false,
			week1:true,
			week2:true,
			isChoose:true,
			noChoose:false,
			dealDate:'',//成交日期
            workDate:'',//生效日期
            billingDate:'',//开单日期
			inputIndex:'',
			alreadyDo:[{name:'已审核',status:true},//高级搜索单据状态
					   {name:'已送审',status:true},
					   {name:'已结案',status:true},
					   {name:'已作废',status:false},
					   {name:'已审核',status:false},
					   {name:'已送审',status:false}],

			businessStatus:[{name:'已生成',status:true},
							{name:'未生成',status:true}],	

			componyTree: [{
                label: '一级 1',
                children: [{
                    label: '二级 1-1',
                    children: [{
                    label: '三级 1-1-1'
                    }]
                }]
                }, {
                label: '一级 2',
                children: [{
                    label: '二级 2-1',
                    children: [{
                    label: '三级 2-1-1'
                    }]
                }, {
                    label: '二级 2-2',
                    children: [{
                    label: '三级 2-2-1'
                    }]
                }]
                }, {
                label: '一级 3',
                children: [{
                    label: '二级 3-1',
                    children: [{
                    label: '三级 3-1-1'
                    }]
                }, {
                    label: '二级 3-2',
                    children: [{
                    label: '三级 3-2-1'
                    }]
                }]
                }],
			tableData: [{
				sequence: '1',
				a:'序号',
				planCode: 'A001',
				planName: '哈哈',
				remark:'12',
				ifAllow:true,
				updateDate:'2017.12.20'
				}, {
				    sequence: '2',
				    planCode: 'A002',
				    planName: '哈哈',
				    remark:'1234',
				    ifAllow:false,
				    updateDate:'2017.12.20'
				}, {
				    sequence: '3',
				    planCode: 'A003',
				    planName: '哈哈',
				    remark:'faf',
				    ifAllow:false,
				    updateDate:'2017.12.20'
				}, {
				    sequence: '4',
				    planCode: 'A004',
				    planName: '哈哈',
				    remark:'fasdg',
				    ifAllow:true,
				    updateDate:'2017.12.20'
				}, {
				    sequence: '5',
				    planCode: 'A005',
				    planName: '哈哈',
				    remark:'fasdg',
				    ifAllow:true,
				    updateDate:'2017.12.20'
				}, {
				    sequence: '6',
				    planCode: 'A006',
				    planName: '哈哈',
				    remark:'fasdg',
				    ifAllow:true,
				    updateDate:'2017.12.20'
				}, {
				    sequence: '7',
				    planCode: 'A007',
				    planName: '哈哈',
				    remark:'fasdg',
				    ifAllow:true,
				    updateDate:'2017.12.20'
				}, {
				    sequence: '8',
				    planCode: 'A008',
				    planName: '哈哈',
				    remark:'fasdg',
				    ifAllow:true,
				    updateDate:'2017.12.20'
				}, {
				    sequence: '9',
				    planCode: 'A009',
				    planName: '哈哈',
				    remark:'fasdg',
				    ifAllow:true,
				    updateDate:'2017.12.20'
				}],
			isEdit:-1,//表格中input编辑
			ifUpdate:-1,//编辑按钮（是否可见）
			//ifSave:-1,//保存按钮（是否可见）
			pageIndex:-1,//分页的当前页码
			totalPage:20,//当前分页总数
      }
  },
  methods:{
		ajaxGet:function(){// 发送ajax请求---get后面括号参数为请求数据的地址
			// 为给定 ID 的 user 创建请求
			let self = this;
			self.axios.get('/user?ID=12345')
			.then(function (response) {//成功，response为返回的数据
			console.log(response);
			})
			.catch(function (error) {//失败
			console.log(error);
			});
		},
		ajaxPost:function(){// 发送ajax请求---post
			let self = this;
			self.axios.post('/user', {
				firstName: 'Fred',
				lastName: 'Flintstone'
			  })
			  .then(function (response) {
				console.log(response);
			  })
			  .catch(function (error) {
				console.log(error);
			  });
		},
        handleNodeClick(data) {//树形结构
            console.log(data);
        },
		handleEdit:function(index){//表格内编辑操作
			this.isEdit=index;//当选中行的索引值与列表中索引值相同，则编辑！
		},
		handleDelete:function(index){//表格内删除操作
			this.tableData.splice(index,1);
		},
		finishEdit: function(index) {//表格内编辑完成事件
			this.isEdit=-1;
		},
		handleCurrentChange:function(val){//获取当前页码
			this.pageIndex=val;
		},
		searchShow:function(){
			let self = this;
			self.highSearchShow = true;
            console.log(self.highSearchShow)
		},
		closeHighSearch:function(){
			let self = this;
			self.highSearchShow = false;
		},
		doEdit:function(){
			let self = this;
			self.canEdit = true;
            self.isChoose = false;
            self.noChoose = true;
		},
		noEdit:function(){
			let self = this;
			self.canEdit = !self.canEdit;
            self.isChoose = true;
            self.noChoose = false;
		},
		cancelweek1:function(){
			let self = this;
			self.week1 = false;
		},
		cancelweek2:function(){
			let self = this;
			self.week2 = false;
		},
		
	},
}
</script>

<style  scoped>
.short-basic-data{
    background: white;
    border: 1px solid #cccccc;
}
/* 搜索框部分開始 */
.search-component{
    background: white;
    border:1px solid rgba()228,228,228,1;
    height: 61px;
    padding-top:15px;
    padding: 15px;
}
.green-btn{
    height: 30px;
    line-height: 30px;
    background-color:rgba(75,207,131,1);
    text-align: center;
    border-radius: 4px;
    margin-right:12px;
    cursor: pointer;
}
.green-btn span{
    color: white;
    font-size: 14px;
}
.search-input{
    border: 1px solid #cccccc;
    border-radius: 3px;
    padding-left: 5px;
    border-right: none;
    margin-left: 10px;
}
.search-input .form-control{
    width:100%;
    height:27px;
    outline: none;
    border:none;
    border-radius:3px 0 0 3px;
}
.search-btn{
    background:#82AAFC;
    height:30px;
    text-align:center;
    line-height:30px;
    border-radius:0 4px 4px 0;
    cursor: pointer;
    margin-left: -1px;
}
.search-btn span{
    color:white;
}
.high-search{
    margin-left: 12px;
}
.high-search span{
    color: white;
    background: #82AAFC;
    height: 30px;
    line-height: 30px;
    text-align: center;
    border-radius: 3px;
    cursor: pointer;
    display: inline-block;
    width: 100%;
}
.gray-btn{
    float: right;
    height: 30px;
    line-height: 30px;
    margin-left: 10px;
    border-radius: 3px;
    background-color:rgba(242,242,242,1);
    text-align: center;
    cursor: pointer;
}
/* 搜索框部分結束 */



/* 重写checkbox */
.el-checkbox__inner{
    width: 24px;
    height: 24px;
    border-radius:50% !important; 
}
.el-checkbox__inner::after{
    -webkit-box-sizing: content-box;
    box-sizing: content-box;
    content: "";
    border: 3px solid #fff;
    border-left: 0;
    border-top: 0;
    height: 11px;
    left: 6px;
    position: absolute;
    top: 1px;
    -webkit-transform: rotate(45deg) scaleY(0);
    transform: rotate(45deg) scaleY(0);
    width: 6px;
    -webkit-transition: -webkit-transform .15s cubic-bezier(.71,-.46,.88,.6) 50ms;
    transition: -webkit-transform .15s cubic-bezier(.71,-.46,.88,.6) 50ms;
    transition: transform .15s cubic-bezier(.71,-.46,.88,.6) 50ms;
    transition: transform .15s cubic-bezier(.71,-.46,.88,.6) 50ms,-webkit-transform .15s cubic-bezier(.71,-.46,.88,.6) 50ms;
    -webkit-transform-origin: center;
    transform-origin: center;
}

/* 重写el-table样式 */
.el-table th {
    white-space: nowrap;
    overflow: hidden;
    user-select: none;
    text-align: left;
    padding: 5px 0;
    text-align: center;
    background-color: #ececec;
}
.el-table td{
    padding: 3px 0;
}
.el-table__body{
    text-align: center;
}
/* 重写el-pagination样式 */
.el-pagination.is-background .btn-next, .el-pagination.is-background .btn-prev, .el-pagination.is-background .el-pager li{
    border-radius: 50%;
}

.text-right{
    text-align: right;
}
.mt-10{
    margin-top: 10px;
}
.el-date-editor.el-input, .el-date-editor.el-input__inner{
    width: 130px;
}
.el-input--suffix .el-input__inner{
    padding-right: 0;
} 

/* 资料列表部分开始 */
.tree-container{
    border-top: 1px solid #ebeef5;
}
.tree-container ul{
    list-style: none;
}

/* 资料列表部分结束 */

/* 高级搜索开始 */
.high-search-container{
    position: fixed;
    right: 0;
    top: 0;
    z-index: 1000;
    width: 25%;
    background: white;
    border-left: 3px solid rgba(50, 50, 50, .2);
    height: 100%;
}
.high-search-header{
    height: 73px;
    line-height: 73px;
    border-bottom: 1px solid #cccccc;
}
.high-search-header .el-col{
    font-size: 20px;
    font-weight: 400;
    text-align: center
}
.super-search{
    color: #169BD5;
}

.search-plan{
    
}
.search-edit span{
    text-align: center;
    cursor: pointer;
    color: #82AAFC;
}
.search-all{    
    cursor: pointer;
}
.this-week{
    padding: 0;
    list-style: none;
    border-bottom: 6px solid #cccccc;
    padding-top:10px;  
}
.this-week li{
    width: 89px;
    height: 30px;
    line-height: 30px;
    border: 1px solid #cccccc;
    border-radius: 3px;
    text-align: center;
    display: inline-block;
    margin-bottom: 20px;
    margin-left: 15px;
    position: relative
}
.this-week li .red-bg{
    position: absolute;
    top: 0;
    left: -15px;
    cursor: pointer;
}
.this-week li .white-one{
    position: absolute;
    top: 11px;
    left: -12px;
    z-index: 10000;
    cursor: pointer;
}
.search-add{  
    cursor: pointer;
    color: #cccccc;
}
.search-num{
    margin-top: 10px;
}
.search-num input{
    height: 32px;
    line-height: 32px;
    border: none;
    outline: none;
}
.data-ul li{
    display: inline-block;
}
.select-logo{
    display: inline-block;
}
.status-ready{
    display: inline-block;
}
.is-select{
    background:#82AAFC;
    width: 23px;
    height: 23px;
    border-radius: 23px;
    text-align: center;
    cursor: pointer;
}
.no-select{
    background:#999999;
    width: 23px;
    height: 23px;
    border-radius: 23px;
    text-align: center;
    cursor: pointer;
}
.data-status{
    font-size: 14px;
}
.data-status .data-ul{
    list-style:none;
}
.data-ul li{
    margin-left: 30px;
    margin-top: 5px;
}
.status-ready{
    margin-left: 5px;
}

.search-depend{
    font-size: 14px;
    padding-top: 5px;
}
.search-depend-after{
    height: 33px;
    padding-left: 30px;
}
.search-box{
    border: 1px solid #cccccc;
    border-radius: 3px;
}
.search-box input{ 
    border:none;
    outline: none;
    height: 26px;
    width: 100%;
}
.logo-p{
    padding-top:3px;
    padding-left: 3px; 
}
.logo-p img{
    width: 23px;
    height: 23px;
}
.search-date{
    margin-top: 5px;
}
.search-date span{
    font-size: 14px;
}

.el-date-editor.el-input[data-v-48e3e0c4], .el-date-editor.el-input__inner[data-v-48e3e0c4]{
    width: 100%;
}
.search-date-after{
    margin-left: 30px;
}
.last-btn{
    margin-top: 10px;
}
.s-btn{
    display: inline-block;
    background: #82AAFC;
    color: white;
    height: 30px;
    width: 90px;
    line-height: 30px;
    text-align: center;
    border-radius: 3px;
}
.r-btn{
    display: inline-block;
    background: rgba(255, 204, 102, 1);
    color: white;
    height: 30px;
    width: 90px;
    line-height: 30px;
    text-align: center;
    border-radius: 3px;
}
.c-btn{
    display: inline-block;
    background: rgba(245, 94, 110, 1);
    color: white;
    height: 30px;
    width: 90px;
    line-height: 30px;
    text-align: center;
    border-radius: 3px;
}
/* 高级搜索结束 */

</style>
