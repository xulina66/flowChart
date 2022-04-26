<!--
 * @Descripttion: 
 * @version: 
 * @Author: xln
 * @Date: 2022-04-22 08:47:02
 * @LastEditors: Please set LastEditors
 * @LastEditTime: 2022-04-25 11:25:27
-->
<template>
  <div class="outContainer">
    <!-- 一列 -->
    <div class="oneRow">
      <div class="top">
        <div style="display:flex;justify-content: space-between;">
          <div>
            <i class="el-icon-video-play"></i>
            <span>{{treeData[0].name}}</span>
          </div>
          <i
            class="el-icon-more"
            style="display: inline-block; margin: 5px 5px 0 10px;  vertical-align: middle;"
          ></i>
        </div>
        <div
          class="branch"
          style="right: 0px;"
          v-for="item in treeData[0].children"
          :key="item.id"
          :style="{right: item.num*(-290)+'px',}"
        >
          <div
            class="turnCorner"
            :style="{width:item.num*290+175+'px',marginLeft:-item.num*290+'px'}"
          ></div>
          <div class="list-item" v-for="(item2,index2) in item.list" :key="item2.id">
            <div class="condition">
              <div class="arrow">
                <div class="line primary-bg" :style="{height:index2>0?'195px':'90px'} "></div>
                <div class="triangle primary-border"></div>
              </div>
              <div class="info" v-show="item.condition">{{item.condition}}</div>
            </div>
            <div class="block-item">
              <div class="title">
                <div>
                  <i class="el-icon-edit"></i>
                  <span>{{item2.name}}</span>
                </div>
                <i class="el-icon-more"></i>
              </div>
              <div class="content">
                <p>{{item2.content}}</p>
                <div>
                  <div></div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- 节点方框 -->
      <template v-for="(item,index) in treeData">
        <div class="list-item" :key="item.id" v-if="index>0">
          <div class="condition">
            <div class="arrow">
              <div class="line" :class="index>1?'primary-bg':'muted-bg'"></div>
              <div class="triangle" :class="index>1?'primary-border':'muted-border'"></div>
            </div>
            <div class="info" v-if="index>1" v-show="item.condition">{{item.condition}}</div>
          </div>
          <div class="block-item">
            <div class="title">
              <div>
                <i class="el-icon-edit"></i>
                <span>{{item.name}}</span>
              </div>
              <i class="el-icon-more"></i>
            </div>
            <div class="content">
              <p>{{item.content}}</p>
              <div>
                <div></div>
              </div>
            </div>
            <!-- 分支 -->
            <div
              class="branch"
              style="right: 0px;"
              v-for="item1 in item.children"
              :key="item1.id"
              :style="{right: item1.num*(-290)+'px',}"
            >
              <div
                class="turnCorner"
                :style="{width:item1.num*290+175+'px',marginLeft:-item1.num*290+'px'}"
              ></div>
              <div class="list-item" v-for="(item2,index2) in item1.list" :key="item2.id">
                <div class="condition">
                  <div class="arrow">
                    <div class="line primary-bg" :style="{height:index2>0?'120px':'195px'} "></div>
                    <div class="triangle primary-border"></div>
                  </div>
                  <div class="info">{{item1.condition}}</div>
                </div>
                <div class="block-item">
                  <div class="title">
                    <div>
                      <i class="el-icon-edit"></i>
                      <span>{{item2.name}}</span>
                    </div>
                    <i class="el-icon-more"></i>
                  </div>
                  <div class="content">
                    <p>{{item2.content}}</p>
                    <div>
                      <div></div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </template>

      <div class="arrow">
        <div class="line muted-bg"></div>
        <div class="triangle muted-border"></div>
      </div>
      <div class="top">
        <i class="el-icon-switch-button"></i>
        <span>主4.结束节点</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "treeComponent",
  components: {},
  data() {
    return {
      topImg: require("@/assets/flow/flow1.png"),
      editImg: require("@/assets/flow/flow2.png"),
      bottomImg: require("@/assets/flow/flow3.png"),
      item1Img: require("@/assets/flow/item1.png"),
      item2Img: require("@/assets/flow/item2.png"),
      item3Img: require("@/assets/flow/item3.png"),
      item4Img: require("@/assets/flow/item4.png"),
      treeData: [
         {
          name: "发起节点1",
          id: 1,
          content:
            " 负责人：信息数据部李小小、开发工程师张大一对符合法规发环境的附加费点金计划",
          children: [
            // 右边第一个分支
            {
              num: 4,
              list: [
                {
                  name: "分支11",
                  id: 3,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                },
                {
                  name: "分支12",
                  id: 4,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                }
              ]
            },
            //右边第二个分支
            {
              num: 5,
              list: [
                {
                  name: "分支21",
                  id: 5,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                },
                {
                  name: "分支22",
                  id: 6,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                }
              ]
            }
          ]
        },
        {
          name: "发起节点1",
          id: 1,
          content:
            " 负责人：信息数据部李小小、开发工程师张大一对符合法规发环境的附加费点金计划",
          children: [
            // 右边第一个分支
            {
              num: 2,
              list: [
                {
                  name: "分支11",
                  id: 3,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                },
                {
                  name: "分支12",
                  id: 4,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                }
              ]
            },
            //右边第二个分支
            {
              num: 3,
              list: [
                {
                  name: "分支21",
                  id: 5,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                },
                {
                  name: "分支22",
                  id: 6,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                }
              ]
            }
          ]
        },
        {
          name: "审批节点2",
          id: 7,
          condition: "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个",
          children: [
            // 右边第一个分支
            {
              num: 0,
              list: [
                {
                  name: "分支31",
                  id: 7,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                },
                {
                  name: "分支32",
                  id: 9,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                }
              ]
            },
            //右边第二个分支
            {
              num: 1,
              list: [
                {
                  name: "分支41",
                  id: 10,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                },
                {
                  name: "分支42",
                  id: 11,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                }
              ]
            }
          ]
        }
      ]
    };
  },
  computed: {},
  watch: {},
  methods: {},
  created() {},
  mounted() {},
  destroyed() {},
  beforeCreate() {},
  beforeMount() {},
  beforeUpdate() {},
  updated() {},
  beforeDestroy() {},
  activated() {}
};
</script>
<style lang='scss' scoped>
div {
  box-sizing: border-box;
}
.outContainer{
  position: relative;
  height: 90vh;
  width: 100%;
  overflow: auto;
}
.oneRow {
  margin-left: 20px;
}
.top {
  width: 230px;
  height: 32px;
  background-color: #70c3fd;
  border-radius: 4px;
  border: solid 1px #70c3fd;
  color: #ffffff;
  font-family: SourceHanSansCN-Medium;
  font-size: 14px;
  line-height: 32px;
  position: relative;
  i {
    font-size: 20px;
    display: inline-block;
    margin: 0 5px 0 10px;
    vertical-align: middle;
  }
}
.arrow {
  width: 230px;
  .line {
    width: 2px;
    margin: 0 auto;
  }
  .triangle {
    width: 0;
    height: 0;
    overflow: hidden;
    font-size: 0;
    line-height: 0;
    border-width: 9px;
    border-style: solid;
    margin: 0 auto;
    margin-bottom: -9px;
  }

  .muted-bg {
    background-color: #dddddd;
    height: 72px;
  }
  .muted-border {
    border-color: #dddddd transparent transparent transparent;
  }
  .primary-bg {
    background-color: #70c3fd;
    height: 120px;
  }
  .primary-border {
    border-color: #70c3fd transparent transparent transparent;
  }
}

.block-item {
  width: 230px;
  height: 150px;
  position: relative;
  font-size: 14px;
  .title {
    width: 230px;
    height: 32px;
    line-height: 32px;
    background-color: #70c3fd;
    border-radius: 4px 4px 0px 0px;
    border: solid 1px #70c3fd;
    color: #ffffff;
    display: flex;
    justify-content: space-between;
    align-content: center;
    align-items: center;
    i {
      display: inline-block;
      margin: 0 5px 0 10px;
      vertical-align: middle;
      font-size: 20px;
    }
  }
  .content {
    border: solid 1px #dddddd;
    border-top: none;
    border-radius: 0px 0px 4px 4px;
    height: 120px;
    width: 230px;
    box-sizing: border-box;
    padding: 15px;
    p {
      padding: 0;
      margin: 0;
      width: 100%;
      display: -webkit-box;
      overflow: hidden;
      text-overflow: ellipsis;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      font-family: SourceHanSansCN-Medium;
      font-size: 14px;
      line-height: 20px;
      color: #666666;
    }
    > div {
    }
  }
}
.list-item {
  .condition {
    position: relative;
  }
  .info {
    width: 240px;
    height: 32px;
    background-color: #ffffff;
    border-radius: 4px;
    border: solid 1px #70c3fd;
    position: absolute;
    box-sizing: border-box;
    bottom: 50px;
    font-family: SourceHanSansCN-Medium;
    font-size: 14px;
    color: #666666;
    line-height: 32px;
    padding: 0 15px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
}
.branch {
  position: absolute;
  top: 50%;
  transform: translateX(100%);
  .turnCorner {
    width: 175px;
    height: 2px;
    background-color: #70c3fd;
  }
  .list-item {
    margin-top: -2px;
    margin-left: 60px;
  }
}
</style>