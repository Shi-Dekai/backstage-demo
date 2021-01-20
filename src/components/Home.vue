<template>
  <div>
    <div class="nav">
      <div class="one menu">菜单1</div>
      <div class="two menu">菜单2</div>
    </div>

    <div style="display: flex">
      <div class="main">
        <button class="BulkLoading" @click="batchAdd">批量添加</button>
        <div class="detail">
          <div class="header">
            <label style="width: 5vw;">
              <input type="checkbox" id="checkbox" v-model="checked" @change="changeAllChecked()">
            </label>
            <div style="width: 20vw;">企业名称</div>
            <div style="width: 12vw;">综合指数</div>
            <div style="width: 12.375vw">创新指数</div>
            <div style="width: 12.375vw">营业收入(万元)</div>
            <div style="width: 12.375vw">税收(万元)</div>
            <div style="width: 12.375vw">固投(万元)</div>
            <div style="width: 5vw">操作</div>
          </div>

          <div class="content" v-for="item in list" :key="item.name">
            <label style="width: 5vw;"><input type="checkbox" :value="item.name" v-model="checkedNames"></label>
            <div style="width: 20vw">{{item.name}}</div>
            <div style="width: 12vw">{{item.synthesize}}</div>
            <div style="width: 12.375vw">{{item.innovate}}</div>
            <div style="width: 12.375vw">{{item.operatingReceipt}}</div>
            <div style="width: 12.375vw">{{item.revenue}}</div>
            <div style="width: 12.375vw">{{item.fixation}}</div>
            <div style="width: 5vw; color: rgb(128,52,38); cursor:pointer" v-if="item.open" @click="add(item.name)"><b>添加</b></div>
            <div style="width: 5vw; color: rgb(200,196,194)" v-else>已添加</div>
          </div>
        </div>

      </div>

      <div class="aside">
        <div style="font-size: 13px; padding: 10px 0 0 10px; color: #803426;">待提交列表（{{toSubmit.length}}）</div>
        <div class="buttons">
          <button class="batch" @click="batchRemove">批量移除</button>
          <button class="empty" @click="empty">清空</button>
          <button class="submit">提交</button>
        </div>

        <div class="detail">
          <div class="header">
            <label style="width: 5vw;">
              <input type="checkbox" id="checkbox1" v-model="checked1" @change="changeAllChecked1()">
            </label>
            <div style="width: 20vw;">企业名称</div>
          </div>
          <div class="content" v-for="(item, index) in toSubmit" :key="index">
            <label style="width: 5vw;"><input type="checkbox" :value="item" v-model="checkedNames1"></label>
            <div style="width: 20vw">{{item}}</div>
            <div style="width: 5vw; cursor: pointer; position: absolute; right: 0" @click="X(item,index)">X</div>
          </div>
        </div>
      </div>
    </div>

  </div>

</template>

<script lang="js">
  export default {
    name: 'Home',
    data(){
      return{
        checked: false,
        checkedNames: [],
        checkedArr: ["阿里巴巴", "腾讯", "百度", '京东', '蚂蚁小微', '网易', '美团点评', '字节跳动'],
        checked1: false,
        checkedNames1: [],
        toSubmit:[],
        list: [
          {name: '阿里巴巴', synthesize: '37.88', innovate: '16.36', operatingReceipt: '8090.8', revenue:'238.32', fixation:'0.00', open: true},
          {name: '腾讯', synthesize: '28.54', innovate: '13.24', operatingReceipt: '6695.4', revenue:'72.63', fixation:'0.00', open: true},
          {name: '百度', synthesize: '29.48', innovate: '17.65', operatingReceipt: '0.1', revenue:'0.00', fixation:'0.00', open: true},
          {name: '京东', synthesize: '9.88', innovate: '15.16', operatingReceipt: '876.2', revenue:'6.15', fixation:'0.00', open: true},
          {name: '蚂蚁小微', synthesize: '19.84', innovate: '14.64', operatingReceipt: '1442.2', revenue:'219.09', fixation:'0.00', open: true},
          {name: '网易', synthesize: '73.96', innovate: '18.86', operatingReceipt: '2082.2', revenue:'4042.18', fixation:'8764.00', open: true},
          {name: '美团点评', synthesize: '29.61', innovate: '14.02', operatingReceipt: '65.8', revenue:'87.09', fixation:'9976.00', open: true},
          {name: '字节跳动', synthesize: '76.58', innovate: '16.25', operatingReceipt: '48505.8', revenue:'3047.99', fixation:'58468.00', open: true}
          ]
      }
    },
    methods:{
      X(name,index){
        this.toSubmit.splice(index, 1)
        this.list.forEach(item=>{
          if(item.name === name){
            item.open = true
          }
        })
      },
      batchRemove(){
        this.checkedNames1.forEach((name)=>{
          this.toSubmit.forEach((item,index)=>{
            if (name === item){
              this.toSubmit.splice(index, 1)
            }
          })
          this.list.forEach(item=>{
            if(item.name === name){
              item.open = true
            }
          })
        })

        this.checkedNames1 = []
      },
      changeAllChecked: function() {
        if (this.checked) {
          this.checkedNames = this.checkedArr
        } else {
          this.checkedNames = []
        }
      },

      changeAllChecked1(){
        if (this.checked1) {
          this.checkedNames1 = this.checkedArr
        } else {
          this.checkedNames1 = []
        }
      },

      add(name){
        console.log('添加成功')
        this.list.forEach(item=>{
          if (item.name === name){
            this.toSubmit.push(item.name)
            item.open = false
          }
        })
        console.log(this.toSubmit)
      },
      empty(){
        this.toSubmit = []
        this.list.forEach(item=>{
          item.open = true
        })
      },
      batchAdd(){
        if (this.checkedNames.length !== 0){
          this.checkedNames.forEach(item =>{
            if (this.toSubmit.indexOf(item) === -1){
              this.toSubmit.push(item)
            }
          })
        }
        this.checkedNames = []

        this.toSubmit.forEach(name=>{
          this.list.forEach(item =>{
            if (item.name === name){
              item.open = false
            }
          })
        })

        console.log(this.toSubmit)
      }
    },

    watch: {
      "checkedNames": function() {
        this.checked = this.checkedNames.length === this.checkedArr.length;
      },
      "checkedNames1": function() {
        this.checked = this.checkedNames.length === this.checkedArr.length;
      }
    }
  }
</script>

<style lang="scss" scoped>
  .aside{
    background: #FCFAF7;
    width: 30vw;
    border: 2px solid rgb(234,234,234);
    border-radius: 2px;
    margin: 30px 10px 0 10px;

    .buttons{
      padding: 20px 10px 10px 10px;
      position: relative;

      >button{
        padding: 4px;
        border: 1px solid #803426;
        border-radius: 2px;
        color: #803426;
        background: white;
      }

      >.empty{
        margin-left: 10px;
      }

      >.submit{
        color: white;
        background: rgb(128,52,38);
        position: absolute;
        right: 10px;
      }
    }
  }

  .detail{
    width: 100%;
    padding: 10px;

    .header{
      padding-left: 30px;
      display: flex;
      justify-content: center;
      align-items: center;
      background: #E8DDD8;
      height: 45px;
    }
    .content{
      position: relative;
      padding-left: 30px;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 40px;
      border-bottom: 1px solid rgb(245,239,230);
    }
  }

  .nav {
    width: 100%;
    height: 45px;
    background: black;
    display: flex;
    justify-content: center;

    > .menu {
      width: 65px;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    > .one {
      background: white;
    }

    > .two {
      color: white;
    }
  }

  .main {
    background: #FCFAF7;
    width: 70vw;
    border: 2px solid rgb(234,234,234);
    border-radius: 2px;
    margin-top: 30px;
    margin-left: 10px;


    > .BulkLoading {
      margin: 10px 10px 0;
      font-size: 12px;
      background: #ffffff;
      color: #803426;
      border: 1px solid #803426;
      width: 70px;
      height: 20px;
      border-radius: 2px;
    }
  }

</style>