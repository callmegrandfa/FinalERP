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

                <el-col :span='2' class="high-search" @click='searchShow'>
                    <span>高级搜索</span>
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
            <el-row>
                <el-col :span='4'>
                    <div class="tree-container">
                        <ul>
                            <li v-for='item in companyTree'>{{item.big}}
                                <ul>
                                    <li v-for='items in item.small'>{{items}}</li>
                                </ul>
                            </li>
                        </ul>
                    </div>
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
                    <el-pagination class="text-right mt-10" background layout="total, prev, pager, next"  :page-count="totalPage" v-on:current-change="handleCurrentChange"></el-pagination>   
                </el-col>
            </el-row>
            <!-- 基础资料表单结束 -->
            <!-- 高級搜索開始 -->
            <div class="high-search-container" v-if='highSearchShow'>
                <ul class="search-ul">
                    <li class="">
                        <span>高级搜索</span>
                    </li>
                    <li class="more-high-search">
                        <span>超级搜索</span>
                    </li>
                    <li class="close-li" @click='closeHighSearch'>
                        <!-- <img src="../assets\layouts\layout2\img\close.png"/> -->
                    </li>
                </ul>

                <div class="search-nav row">
                    <div class="search-plan col-md-8">
                        <span>查询方案</span>
                    </div>
            
                    <div class="search-edit col-md-2">
                        <span @click='doEdit' 
                              v-if='!canEdit'>编辑</span>

                        <span @click='noEdit'
                              v-if='canEdit'>确定</span>
                    </div>
            
                    <div class="search-all col-md-2">
                        <span style="color:#CCCCCC">全部</span>
                        <!-- <img src="../assets\layouts\layout2\img\down.png"/> -->
                    </div>
                </div>

                <ul class="this-week">
                    <li v-if='week1'>
                        <span>本周未审核</span>
                        <!-- <div @click='cancelweek1'>
                            <img class="white-one" 
                                src="../assets\layouts\layout2\img\whiteone.png"
                                v-if='canEdit'>

                            <img class="red-bg" 
                                src="../assets\layouts\layout2\img\redcircle.png" 
                                v-if='canEdit'>
                        </div> -->
                    </li>

                    <li v-if='week2'>
                        <span>本周已审核</span>
                        <!-- <div @click='cancelweek2'>
                            <img class="white-one" 
                                src="../assets\layouts\layout2\img\whiteone.png"
                                v-if='canEdit'>

                            <img class="red-bg" 
                                src="../assets\layouts\layout2\img\redcircle.png" 
                                v-if='canEdit'>
                        </div> -->
                    </li>
                </ul>

                <div class="row">
                    <div class="search-condition col-md-10">
                        <span>查询条件</span>
                    </div>
            
                    <div class="search-add col-md-2">
                        <span>添加</span>
                    </div>
                </div>

                <div class="search-num">
                    <input type="text" 
                    class="form-control" 
                    placeholder="请输入查询单号..." 
                    style="width:80%">
                </div>

                <div class="data-status row">
                        <div class="col-md-3" style='font-size:12px;margin-top:5px;margin-left:15px'>
                            <span>单据状态</span>
                        </div>
            
                        <ul  class="data-ul col-md-8 row" style="list-style:none">
                            <!-- <li v-for='item in alreadyDo'>
                                <el-checkbox v-model="item.status" ></el-checkbox>
            
                                <div class="status-ready">
                                    <span>{{item.name}}</span>
                                </div>
                            </li> -->
                        </ul>
                </div>

                <div class="business-status row">
                    <div class="col-md-3" style='font-size:12px;margin-top:5px;margin-left:15px'>
                        <span>业务状态</span>
                    </div>
            
                    <ul  class="data-ul col-md-8 row" style="list-style:none">
                        <!-- <li v-for='item in businessStatus'>
                            <el-checkbox v-model="item.status" ></el-checkbox>
        
                            <div class="status-ready">
                                <span>{{item.name}}</span>
                            </div>
                        </li> -->
                    </ul>
                </div>
                
                <ul class="search-depend">
                    <li class="row">
                        <div class="col-md-3">
                            <span>货号</span>
                        </div>
                        <div class="search-box col-md-7">
                            <!-- <img src="../assets\layouts\layout2\img\searchLogo.png" alt=""> -->
                            <input type="text" placeholder="请输入货号">
                        </div> 
                    </li>
                    <li class="row">
                        <div class="col-md-3">
                            <span>供应商</span>
                        </div>
                        <div class="search-box col-md-7">
                            <!-- <img src="../assets\layouts\layout2\img\searchLogo.png" alt=""> -->
                            <input type="text" placeholder="请输入供应商">
                        </div> 
                    </li>
                    <li class="row">
                        <div class="col-md-3">
                            <span>仓库</span>
                        </div>
                        <div class="search-box col-md-7">
                            <!-- <img src="../assets\layouts\layout2\img\searchLogo.png" alt=""> -->
                            <input type="text" placeholder="请输入仓库">
                        </div> 
                    </li>
                    <li class="row">
                        <div class="col-md-3">
                            <span>PO单号</span>
                        </div>
                        <div class="search-box col-md-7">
                            <!-- <img src="../assets\layouts\layout2\img\searchLogo.png" alt=""> -->
                            <input type="text" placeholder="请输入PO单号">
                        </div> 
                    </li>
                    <li class="row">
                        <div class="col-md-3">
                            <span>PO类型</span>
                        </div>
                        <div class="search-box col-md-7">
                            <!-- <img src="../assets\layouts\layout2\img\searchLogo.png" alt=""> -->
                            <input type="text" placeholder="请输入PO类型">
                        </div> 
                    </li>
                    <li class="row">
                        <div class="col-md-3">
                            <span>PO品牌</span>
                        </div>
                        <div class="search-box col-md-7">
                            <!-- <img src="../assets\layouts\layout2\img\searchLogo.png" alt=""> -->
                            <input type="text" placeholder="请输入PO品牌">
                        </div> 
                    </li>
                </ul>
                
                <div class="search-date">
                    <ul>
                        <li class="row">
                            <div class="col-md-3">
                                <span>开单日期</span>
                            </div>

                            <div class="choose-date col-md-8">
                                <!-- <div class="choose-start">
                                    <input type="text" placeholder="开始..."/>
                                    <img src="../assets\layouts\layout2\img\dateLogo.png" alt="">
                                </div>
                                <div class="choose-end">
                                    <input type="text" placeholder="结束..."/>
                                    <img src="../assets\layouts\layout2\img\dateLogo.png" alt="">
                                </div> -->
                                <el-date-picker
                                v-model="data1"
                                type="date"
                                placeholder="选择日期">
                              </el-date-picker>
                              <el-date-picker
                              v-model="data1"
                              type="date"
                              placeholder="选择日期">
                            </el-date-picker>
                            </div>
                        </li>

                        <li class="row">
                            <div class="col-md-3">
                                <span>生效日期</span>
                            </div>

                            <div class="choose-date col-md-8">
                                <el-date-picker
                                v-model="data1"
                                type="date"
                                placeholder="选择日期">
                              </el-date-picker>
                              <el-date-picker
                              v-model="data1"
                              type="date"
                              placeholder="选择日期">
                            </el-date-picker>
                            </div>
                        </li>

                        <li class="row">
                            <div class="col-md-3">
                                <span>交货日期</span>
                            </div>

                            <div class="choose-date col-md-8">
                                <div class="choose-start">
                                    <input type="text" placeholder="开始..."/>
                                    <!-- <img src="../assets\layouts\layout2\img\dateLogo.png" alt=""> -->
                                </div>
                                <div class="choose-end">
                                    <input type="text" placeholder="结束..."/>
                                    <!-- <img src="../assets\layouts\layout2\img\dateLogo.png" alt=""> -->
                                </div>
                            </div>
                        </li>
                    </ul>
                </div>

                <div class="last-btn">
                    <ul class="row">
                        <li class="col-md-3">
                            <span class="s-btn">查询</span>
                        </li>

                        <li class="col-md-3">
                            <span class="r-btn">重置</span>
                        </li>

                        <li class="col-md-3">
                            <span class="c-btn">取消</span>
                        </li>
                    </ul>
                </div>
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
         highSearchShow:false,
			canEdit:false,
			week1:true,
			week2:true,
			isChoose:true,
			noChoose:false,
			data1:'',
			inputIndex:'',
			alreadyDo:[{name:'已审核',status:true},//高级搜索单据状态
					   {name:'已送审',status:true},
					   {name:'已结案',status:true},
					   {name:'已作废',status:false},
					   {name:'已审核',status:false},
					   {name:'已送审',status:false}],

			businessStatus:[{name:'已生成',status:true},
							{name:'未生成',status:true}],	

			companyTree:[{//左边树形
				big:'上级1',
				small:['下级1','下级2','下级3','下级4']
				},{
					big:'上级2',
					small:['下级1','下级2','下级3','下级4']
				},{
					big:'上级3',
					small:['下级1','下级2','下级3','下级4']
				}
			],
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
}
</script>

<style  scoped>
.short-basic-data{
    background: white;
    border: 1px solid #cccccc;
}
.tree-container{
    background: white;
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
    background: #82AAFC;
    height: 30px;
    line-height: 30px;
    text-align: center;
    border-radius: 3px;
    cursor: pointer;
    margin-left: 12px;
}
.high-search span{
    color: white;
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
.el-checkbox__inner::after{
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

/* 资料列表部分结束 */
.high-search-container{
    position: fixed;
    right: 0;
    top: 0;
    z-index: 1000;
    float: right;
    width: 30%;
    background: white;
    border-left: 3px solid rgba(50, 50, 50, .2);
    height: 100%;
}

.search-ul{
    list-style: none;
    padding: 0;
    height: auto;
    border-bottom: 1px solid #999999;
}
.search-ul li{
    display: inline-block;
    font-size: 18px;
    margin-left: 20px;
    margin-top: 25px;
    height: 37px;
    line-height: 37px;
    margin-bottom: 3px;
}
.search-ul .more-high-search{
    color: #82AAFC;
    cursor: pointer;
}
.search-ul .close-li{
    float:right;
    cursor: pointer;
    margin-right:15px;
}
.search-plan{
    padding-left: 30px;
}
.search-edit{
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
.search-condition{
    padding-left: 30px;
}
.search-add{  
    cursor: pointer;
    color: #cccccc;
}
.search-num{
    margin-top: 10px;
    padding-left:15px; 
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
    padding-top:10px; 
}
.data-status .data-ul{
    padding: 0;
}
.business-status{
    padding-top:10px; 
}
.business-status .data-ul{
    padding: 0;
}
.status-ready{
    margin-left: 5px;
}
.data-ul li{
    margin-left: 30px;
    margin-top: 5px;
}

.search-depend{
    list-style: none;
    padding: 0;

}
.search-depend li{
    padding-left: 15px;
    margin-top: 10px;
}
.search-box{
    position: relative;
    height: 33px;
    border: 1px solid #cccccc;
    line-height: 33px;
    margin-left: 20px;
    border-radius: 3px;
}
.search-box input{
    position: absolute;
    left: 40px;
    top: 3px;
    border:none;
    outline: none;
    height: 26px;
}
.search-date ul{
    list-style: none;
    padding: 0;
}
.search-date ul li{
    padding-left:15px; 
    margin-top: 5px;
}
.choose-start,.choose-end{
    display: inline-block;   
    position: relative;
    border: 1px solid #cccccc;
    border-radius: 3px;
    height: 30px;
    line-height: 25px;
     
}
.choose-date{
    margin-left: 5px;
}
.choose-date input{
   width: 125px;
   border: none;
   outline: none;
   padding-left:5px;
}
.choose-date input::-webkit-inner-spin-button {
     visibility: hidden;
 }
.choose-date img{
    position: absolute;
    right: 5px;
    top:3px;
}
.last-btn{
    margin-top: 18px;
}

.last-btn ul{
    padding:0;
    list-style: none;
    padding-left: 15px;
}
.last-btn ul li{
    display: inline-block;
    cursor: pointer;
}
.last-btn  ul li span{
    width: 89px;
    height: 30px;
    text-align: center;
    line-height: 30px;
    display: inline-block;
    border-radius: 3px;
}
.s-btn{
    background: #82AAFC;
    color: white;
}
.r-btn{
    background: rgba(255, 204, 102, 1);
    color: white;
}
.c-btn{
    background: rgba(245, 94, 110, 1);
    color: white;
}
.el-table__row input[disabled]{
    background: none;
    border: none;
}

</style>
