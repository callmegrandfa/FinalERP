<template>
  <div class="groupManager">
    <el-container>
      <!-- 主表或查询条件信息 -->
      <el-header class="header-input mt-10">
        <el-row :gutter="20">
          <el-col :xs="12" :sm="6" :md="6" :lg="5">
            <el-input placeholder="恒康科技" v-model="input3" size="medium">
                <template slot="prepend">集团编码</template>
            </el-input>
          </el-col>
          <el-col :xs="12" :sm="6" :md="6" :lg="5">
            <el-input placeholder="30" v-model="input3" size="medium">
                <template slot="prepend">集团名称</template>
            </el-input>
          </el-col>
          <el-col :xs="12" :sm="6" :md="6" :lg="5">
            <el-input placeholder="lomen" v-model="input3" size="medium">
                <template slot="prepend">集团全称</template>
            </el-input>
          </el-col>
          <el-col :xs="12" :sm="6" :md="6" :lg="5">
            <el-input placeholder="请输入助记码" v-model="input3" size="medium">
                <template slot="prepend">助记码</template>
            </el-input>
          </el-col>
        </el-row>
        <el-row :gutter="20">
          <el-col :xs="12" :sm="6" :md="6" :lg="5">
            <div class="el-input el-input--medium el-input-group el-input-group--prepend">
              <label class="label-title">行政地区	&nbsp&nbsp&nbsp&nbsp</label>
              <el-select v-model="value" placeholder="请选择">
                <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value"></el-option>
              </el-select>
            </div>
          </el-col>
          <el-col :xs="12" :sm="6" :md="6" :lg="5">
            <div class="el-input el-input--medium el-input-group el-input-group--prepend">
              <label class="label-title">启用会计月份</label>
              <el-select v-model="value" placeholder="请选择">
                <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value"></el-option>
              </el-select>
            </div>
          </el-col>
          <el-col :xs="12" :sm="6" :md="6" :lg="5">
            <div class="el-input el-input--medium el-input-group el-input-group--prepend">
              <label class="label-title">本位币种&nbsp&nbsp&nbsp&nbsp</label>
              <el-select v-model="value" placeholder="请选择">
                <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value"></el-option>
              </el-select>
            </div>
          </el-col>
          <el-col :xs="12" :sm="6" :md="6" :lg="5">
            <el-input placeholder="请输入助记码" v-model="input3" size="medium">
                <template slot="prepend">行业</template>
            </el-input>
          </el-col>
        </el-row>
        <el-row :gutter="20">
          <el-col :xs="12" :sm="6" :md="6" :lg="5">
            <el-input placeholder="恒康科技" v-model="input3" size="medium">
                <template slot="prepend">电话</template>
            </el-input>
          </el-col>
          <el-col :xs="12" :sm="6" :md="6" :lg="5">
            <el-input placeholder="30" v-model="input3" size="medium">
                <template slot="prepend">传真</template>
            </el-input>
          </el-col>
          <el-col :xs="10" :sm="8" :md="6" :lg="5">
            <el-input placeholder="请录入地址" v-model="input3" size="medium">
                <template slot="prepend">地址</template>
            </el-input>
          </el-col>
          <el-col :xs="10" :sm="8" :md="6" :lg="5">
            <el-input placeholder="请录入备注信息" v-model="input3" size="medium">
                <template slot="prepend">备注</template>
            </el-input>
          </el-col>
        </el-row>
        <el-row>
          <el-checkbox v-model="checked">允许使用</el-checkbox>
          <el-button class="ml-10" size="medium" type="primary">查询</el-button>
        </el-row>
      </el-header>
      <!-- 子表或列表信息 -->
      <el-main class="bg-white mt-10">
        <el-container>
          <!-- 按钮组 -->
          <el-header>
            <el-button size="medium" type="primary">修改</el-button>
            <el-button size="medium" type="primary">保存</el-button>
            <el-button class="fr" size="medium" type="default">打印</el-button>
          </el-header>
          <el-container class="mt-10">
            <!-- 树结构 -->
            <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
              <el-menu :default-openeds="['1', '3']"  background-color="#e4e4e4" text-color="#666666" active-text-color="#409eff">
                <el-submenu index="1">
                  <template slot="title"><i class="el-icon-message"></i>导航一</template>
                  <el-menu-item-group>
                    <template slot="title">分组一</template>
                    <el-menu-item index="1-1">选项1</el-menu-item>
                    <el-menu-item index="1-2">选项2</el-menu-item>
                  </el-menu-item-group>
                  <el-menu-item-group title="分组2">
                    <el-menu-item index="1-3">选项3</el-menu-item>
                  </el-menu-item-group>
                  <el-submenu index="1-4">
                    <template slot="title">选项4</template>
                    <el-menu-item index="1-4-1">选项4-1</el-menu-item>
                  </el-submenu>
                </el-submenu>
              </el-menu>
            </el-aside>
            <!-- 表格 -->
            <el-main class="container-table">
              <el-table :data="tableData3" height="250" border style="width: 100%">
                <el-table-column prop="date"  label="日期" width="180"></el-table-column>
                <el-table-column prop="name" label="姓名" width="180"></el-table-column>
                <el-table-column prop="address" label="地址"></el-table-column>
              </el-table>
            </el-main>
          </el-container>
        </el-container>
      </el-main>
      <el-footer class="bg-white footer-input">
        <el-collapse v-model="activeNames">
          <el-collapse-item title="审计信息" name="1">
            <el-row :gutter="20">
              <el-col :xs="12" :sm="6" :md="6" :lg="5">
                <el-input placeholder="恒康科技" v-model="input3" size="medium">
                    <template slot="prepend">集团编码</template>
                </el-input>
              </el-col>
              <el-col :xs="12" :sm="6" :md="6" :lg="5">
                <el-input placeholder="30" v-model="input3" size="medium">
                    <template slot="prepend">集团名称</template>
                </el-input>
              </el-col>
              <el-col :xs="12" :sm="6" :md="6" :lg="5">
                <el-input placeholder="lomen" v-model="input3" size="medium">
                    <template slot="prepend">集团全称</template>
                </el-input>
              </el-col>
              <el-col :xs="12" :sm="6" :md="6" :lg="5">
                <el-input placeholder="请输入助记码" v-model="input3" size="medium">
                    <template slot="prepend">助记码</template>
                </el-input>
              </el-col>
            </el-row>
          </el-collapse-item>
        </el-collapse>
      </el-footer>
    </el-container>
  </div>
</template>

<script>
export default {
  name: 'groupManager',
  data () {
    return {
      input3:'',
      options: [{
          value: '选项1',
          label: '黄金糕'
        }, {
          value: '选项2',
          label: '双皮奶'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }],
        value: '',
        checked: true,
        tableData3: [{
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-08',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-06',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-07',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }],
        activeNames: ['1']
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
body{
  padding: 0;
  margin: 0;
}
.fr{
  float: right;
}
.mt-10{margin-top: 10px;}
.ml-10{margin-left: 10px;}
.text-right{
  text-align: right;
}
.el-col{
   margin-bottom: 20px;
}
.groupManager{
  height: 800px;
  background: #eef1f5;
}
.header-input .el-input-group__prepend,.header-input .el-input__inner{
  background: #fff;
  border:none;
}
.groupManager .el-header,.groupManager .el-footer{
  height: auto!important;
}
.label-title{
    padding: 0 20px;
    width: 1px;
    color: #909399;
    background: #fff;
    vertical-align: middle;
    display: table-cell;
    position: relative;
    border-radius: 4px;
    white-space: nowrap;
}
.el-checkbox__inner {
    display: inline-block;
    position: relative;
    border: 1px solid #dcdfe6;
    border-radius: 2px;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    width: 22px;
    height: 22px;
    border-radius: 50%;
    background-color: #fff;
    z-index: 1;
    -webkit-transition: border-color .25s cubic-bezier(.71,-.46,.29,1.46),background-color .25s cubic-bezier(.71,-.46,.29,1.46);
    transition: border-color .25s cubic-bezier(.71,-.46,.29,1.46),background-color .25s cubic-bezier(.71,-.46,.29,1.46);
}
.el-checkbox__inner::after {
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
    width: 5px;
    -webkit-transition: -webkit-transform .15s cubic-bezier(.71,-.46,.88,.6) 50ms;
    transition: -webkit-transform .15s cubic-bezier(.71,-.46,.88,.6) 50ms;
    transition: transform .15s cubic-bezier(.71,-.46,.88,.6) 50ms;
    transition: transform .15s cubic-bezier(.71,-.46,.88,.6) 50ms, -webkit-transform .15s cubic-bezier(.71,-.46,.88,.6) 50ms;
    transition: transform .15s cubic-bezier(.71,-.46,.88,.6) 50ms,-webkit-transform .15s cubic-bezier(.71,-.46,.88,.6) 50ms;
    -webkit-transform-origin: center;
    transform-origin: center;
}
.bg-white{
  background: #fff;
}
.groupManager .el-submenu__title{
  height: 40px;
  line-height: 40px;
}
.groupManager .el-submenu .el-menu-item{
  height: 36px;
  line-height: 36px;
}
.footer-input .el-input-group__prepend{
  background: #fff;
}
.el-collapse-item__header{
  font-size: 16px;
}
.container-table{
  padding: 0 20px 0 40px;
}
</style>
