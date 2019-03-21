<template>
  <div>
    <div>
      <!-- <span class="demonstration">场景: 今日大雨</span> -->
      <el-carousel :interval="4000" type="card" height="100px">
    <el-carousel-item v-for="item in 6" :key="item">
      <h3>{{ item }}</h3>
    </el-carousel-item>
  </el-carousel>
    </div>
    <!-- <span class="demonstration">人物等级</span> -->
    <div>
      <el-steps :active="4" simple>
        <el-step title="雨之气" icon="el-icon-edit"></el-step>
        <el-step title="雨者" icon="el-icon-upload"></el-step>
        <el-step title="雨灵" icon="el-icon-picture"></el-step>
        <el-step title="雨皇" icon="el-icon-edit"></el-step>
        <el-step title="雨尊" icon="el-icon-upload"></el-step>
        <el-step title="雨帝" icon="el-icon-picture"></el-step>
      </el-steps>
    </div>
    <el-container>
      <el-aside width="150px">
          <br>
          <el-button width= "150px">出手</el-button>
          <br/>
          <br>
        <el-table
          ref="multipleTable"
          :data="weaponName"
          tooltip-effect="dark"
          style="width: 110px"
          @selection-change="handleSelectionChange"
        >
          <el-table-column type="selection" width="50"></el-table-column>
          <el-table-column label="武器" width="60">
            <template slot-scope="scope">{{ scope.row.name }}</template>
          </el-table-column>
        </el-table>
        <span></span>
            <br/>
            <br/>
        <el-table
          ref="multipleTable"
          :data="skillName"
          tooltip-effect="dark"
          style="width: 110px"
          @selection-change="handleSelectionChange"
        >
          <el-table-column type="selection" width="50"></el-table-column>
          <el-table-column label="技能" width="60">
            <template slot-scope="scope">{{ scope.row.name }}</template>
          </el-table-column>
        </el-table>
      </el-aside>
      <el-container>
        <el-header>
          <div >
            <span class="left">角色：虞大叶</span>
            <span class="right" >Monster：破儿</span>
          </div>
        </el-header>
        <el-main>
          <el-card class="box-card">
            <div v-for="o in 4" :key="o" class="text item">{{'列表内容 ' + o }}</div>
          </el-card>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
export default {
  name: "Battle",
  data() {
    return {
      value1: 0,
      weaponName: [
        {
          name: "武器1"
        },
        {
          name: "武器2"
        },
        {
          name: "武器3"
        },
        {
          name: "武器4"
        },
        {
          name: "武器5"
        }
      ],
      skillName: [
        {
          name: "技能1"
        },
        {
          name: "技能2"
        },
        {
          name: "技能3"
        },
        {
          name: "技能4"
        },
        {
          name: "技能5"
        }
      ]
    };
  }
};
</script>
<style>
.el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 100px;
    margin: 0;
  }
  
  .el-carousel__item:nth-child(2n) {
    background-color: #99a9bf;
  }
  
  .el-carousel__item:nth-child(2n+1) {
    background-color: #d3dce6;
  }
.text {
  font-size: 14px;
}

.item {
  padding: 18px 0;
}

.box-card {
  width: 100%;
  height: 100%;
}
.el-header {
  background-color: #b3c0d1;
  color: #333;
  line-height: 60px;

  
}
.left{
   float:left
  }
.right{
   float:right
  }
.el-main{
    height: 100%;
}
.el-aside {
  color: #333;
}
</style>
