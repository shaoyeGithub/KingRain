<template>
  <div>
    <div>
      <!-- <span class="demonstration">场景: 今日大雨</span> -->
      <!-- <el-carousel :interval="4000" type="card" height="100px">
    <el-carousel-item v-for="item in 3" :key="item">
      <h3>{{ item }}</h3>
    </el-carousel-item>
  </el-carousel> -->
    </div>
    <!-- <span class="demonstration">人物等级</span> -->
    <div>
      <el-steps :active= level simple>
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
          <el-button width= "150px" @click.native = "getBattleInf">出手</el-button>
          <br/>
          <br>
        <el-table
          :data="weaponName"
          tooltip-effect="dark"
          style="width: 180px"
          :row-class-name="tableRowClassName"
        >
          <!-- <el-table-column type="selection" width="50"></el-table-column> -->
          <el-table-column label="武器" width="180">
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
          style="width: 180px"
          :row-class-name="tableRowClassName1"
          @selection-change="handleSelectionChange"
        >
          <el-table-column type="selection" width="50"></el-table-column>
          <el-table-column label="技能" width="130">
            <template slot-scope="scope">{{ scope.row.name }}</template>
          </el-table-column>
        </el-table>
      </el-aside>
      <el-container>
        <el-header>
          <div >
            <span class="left" v-bind="Hero">角色：{{Hero.currentUser}}  ----  HP : {{Hero.HP_Hero}}</span>

            <el-dropdown>
      <span class="el-dropdown-link">
        {{BatInf}}<i class="el-icon-arrow-down el-icon--right"></i>
      </span>
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item icon="el-icon-plus" @click.native = 'choseBattle1'>第一关</el-dropdown-item>
        <el-dropdown-item icon="el-icon-circle-plus" @click.native = 'choseBattle2'>第二关</el-dropdown-item>
        <el-dropdown-item icon="el-icon-check" @click.native = 'choseBattle3'>第三关</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>

            <span class="right" v-bind="Mon" >Monster：{{Mon.Mon_name}}  ---- HP :{{Mon.HP_Mon}}</span>
          </div>
        </el-header>
        <el-main>
          <el-card class="box-card">
            <div v-for="item in log" :key= "item.id" class="text_item">{{item}}</div>
          </el-card>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>

import qs from 'qs'
export default {
  name: "Battle",
  data() {
    return {
      value1: 0,
      turn: 1,
      Hero:{
        currentUser: '虞大叶',
        HP_Hero:0,
      },
      weaponOfindex:[1,0,0],
      skillOfindex:[1,0,0],
      log:[],
      BatInf:'开始战斗',
      level:3,
      startTurn:1,
      sid:-1,
      useSkillName:'',
      Mon:{
        Mon_name:'',
        HP_Mon:0,
      },
      
      weaponName: [
        {
          name: "压制之刃【加攻击】"
        },
        {
          name: "天堂之戟【每次击打加护甲】"
        },
        {
          name: "虚灵刀【吸血：每轮攻击回复造成伤害的50%】"
        },
      ],
      multipleSelection: [],
      skillName: [
        {
          name: "魔龙之心【回血】"
        },
        {
          name: "虚空之甲【护甲永久加成4点】"
        },
        {
          name: "魔王降临【连击】"
        },
      ]
    };
    
  },
  mounted () {
    this.initUser()
  },
  methods:{
    tableRowClassName({row, rowIndex}) {
        
        // console.log(this.weaponOfindex[rowIndex])

        
        if (this.weaponOfindex[rowIndex] === 0) {
          return 'warning-row';
        }
        // } else if (rowIndex === 3) {
        //   return 'success-row';
        // }
        return '';
      },
       tableRowClassName1({row, rowIndex}) {
        
        // console.log(this.weaponOfindex[rowIndex])

        
        if (this.skillOfindex[rowIndex] === 0) {
          return 'warning-row';
        }
        // } else if (rowIndex === 3) {
        //   return 'success-row';
        // }
        return '';
      },
    initUser()
    {
        console.log('初始化')
         var that = this
         var init = that.turn
         this.$axios.get('/start').then(function (response) {
          console.log(response.data)
          that.Hero.HP_Hero = response.data[0].hp
          that.level = response.data[0].level -1
          that.attackByHero = response.data[0].attack
          that.Mon.HP_Mon = response.data[1].hp
          that.Mon.Mon_name = response.data[1].name
          that.attackByMonster = response.data[1].attack
          that.weaponOfindex = response.data[0].equipments
          that.skillOfindex = response.data[0].skills

          // console.log(that.weaponOfindex[0])
          // console.log(that.skillOfindex )
      }).catch(function (error) {
        console.log(error)
        that.$message('初始化失败')
      })
    },
    getBattleInf(){
        console.log('获取战斗信息')
         var that = this
         
         if(that.BatInf === '开始战斗')
         {
           this.$message.error('请先开始战斗');
           return;
         }

         var url = '/skill?sid=' + that.sid
         console.log("向后台发送技能"+that.sid)
         this.$axios.get(url
      ).then(function (response) {
          console.log(response.data)
          that.Hero.HP_Hero = response.data[0].hp
          if(that.level < response.data[0].level)
             that.$message({
              message: '恭喜你，升级了',
              type: 'success'
          })
          that.level = response.data[0].level 
          that.attackByHero = response.data[0].attack
          that.Mon.HP_Mon = response.data[1].hp
          that.Mon.Mon_name = response.data[1].name
          that.attackByMonster = response.data[1].attack 
          
          
          // var id = that.log.length
          var str1 = "虞大叶使用  " + that.useSkillName +"  攻击  "+ that.Mon.Mon_name + " 造成 "+response.data[2].monsterBloodLose +" 伤害" + '\n'
          var str =  "    与此同时  "+that.Mon.Mon_name+"  使用  " +that.useSkillName +"  攻击虞大叶造成  "+response.data[2].heroBloodLose +" 伤害" + '\n'
          
          var item = {id:that.log.length +1,inf:str1}
          var item1 = {id:that.log.length +2,inf:str}
          that.log.push(item)
          that.log.push(item1)

          if(response.data[2].over)
             that.$notify({
          title: '战斗',
          message: '战斗结束，请开始第二关',
          type: 'warning'
        });
          
      }).catch(function (error) {
        console.log(error)
        that.$message('出手失败')
      })
      
      this.$refs.multipleTable.clearSelection();

    },
    choseBattle1(){
       this.startTurn = 1
       this.BatInf = '第一关'
       this.StartBattle()
    },
    choseBattle2(){
       this.startTurn = 2
       this.BatInf = '第二关'
       this.StartBattle()
    },
    choseBattle3(){
       this.startTurn = 3
       this.BatInf = '第三关'
       this.StartBattle()
    },
    StartBattle(){
      var that = this
      var url = '/loop?lid='+this.startTurn
      console.log(url)
      this.$axios.get(url
      ).then(function (response) {
          console.log(response.data)
          that.$message(that.BatInf)
          // that.initUser()
          that.Hero.HP_Hero = response.data[0].hp
          that.level = response.data[0].level -1
          that.attackByHero = response.data[0].attack
          that.Mon.HP_Mon = response.data[1].hp
          that.Mon.Mon_name = response.data[1].name
          that.attackByMonster = response.data[1].attack
          that.weaponOfindex = response.data[0].equipments
          that.skillOfindex = response.data[0].skills
      }).catch(function (error) {
        console.log(error)
        that.$message('出手失败')
      })
    },
    handleSelectionChange(val) {
      
        this.multipleSelection = val;
        // console.log(this.multipleSelection.length)
        if(this.multipleSelection.length > 0)
        {
          if(this.multipleSelection[0].name === "虚空之甲【护甲永久加成4点】")
          {
              this.sid = 2
              this.useSkillName = "虚空之甲【护甲永久加成4点】"
          }
          else if (this.multipleSelection[0].name === "魔王降临【连击】")
          {
            this.sid = 3
            this.useSkillName = "魔王降临【连击】"
          }       
          else if (this.multipleSelection[0].name === "魔龙之心【回血】")
          {
            this.sid = 1
            this.useSkillName = "魔龙之心【回血】"
          }  
              
        }
        // console.log(this.multipleSelection.length)
        // this.multipleSelection.length = 0
        // console.log(this.multipleSelection.length)
        // console.log("选择技能："+this.sid)
      }
  }
}
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
.el-table .warning-row {
    background: oldlace;
  }

  .el-table .success-row {
    background: #f0f9eb;
  }
.left{
   float:left;
  }
.right{
   float:right
  }
.text_item{
  float:left;
  word-wrap:break-word
}
.el-main{
    height: 100%;
}
.el-aside {
  color: #333;
}
</style>
