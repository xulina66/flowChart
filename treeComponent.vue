<!--
 * @Descripttion: 
 * @version: 
 * @Author: xln
 * @Date: 2022-04-22 08:47:02
 * @LastEditors: Please set LastEditors
 * @LastEditTime: 2022-05-09 16:40:54
-->
<template>
  <div>
    <drawer-common
      :showFooter="false"
      :title="flowTitle"
      popupWidth="90%"
      :visible="flowVisible"
      @save="flowComfirm"
      @Cancel="flowVisible=false"
      @handleClose="flowVisible=false"
    >
      <template slot="rightTitle">
        <div class="breadcrumb">
          <!-- <div>
            <span style="color: #666666;">流程版本(V1)</span>
            <p>未启用</p>
          </div>
          <div class="line">
            <img src="../../../../assets/flow/top1.png" alt />
            <span>版本管理</span>
          </div>-->
          <div class="line">
            <img src="../../../../assets/flow/top2.png" alt />
            <span>保存</span>
          </div>
          <!-- <div class="line">
            <img src="../../../../assets/flow/top3.png" alt />
            <span>启用流程</span>
          </div>-->
        </div>
      </template>
      <div class="treeContainer">
        <div class="bigTree">
          <!-- 左边流程图 -->
          <div class="outContainer">
            <!-- 一列 -->
            <div class="oneRow">
              <div class="top">
                <div style="display:flex;justify-content: space-between;">
                  <div style="flex: 1;" @click="showRight('startNode',treeData[0],0)">
                    <i class="el-icon-video-play"></i>
                    <span>{{treeData[0].name}}</span>
                  </div>
                  <i
                    class="el-icon-more"
                    style="display: inline-block; padding: 5px 5px 0 10px;  vertical-align: middle;"
                    @mouseenter="changeShow(treeData[0])"
                  ></i>
                </div>
                <!-- crud按钮 -->
                <div
                  class="btnCotent"
                  :class="(showNode&&showNodeId==treeData[0].id)?'showNode':'hideNode'"
                  @mouseleave="showNode=false"
                >
                  <div @click="addNode('startNode',treeData[0],0)">新增节点</div>
                  <div @click="addBrach(treeData[0])">新增分支</div>
                </div>
                <!-- 分支 -->
                <div
                  class="branch"
                  style="right: 0px;"
                  v-for="(item,index) in treeData[0].children"
                  :key="item.id"
                  :style="{right: item.num*(-290)+'px',zIndex:20-index}"
                >
                  <div
                    class="turnCorner"
                    :style="{width:item.num*290+175+'px',marginLeft:-item.num*290+'px'}"
                  ></div>
                  <div class="list-item" v-for="(item2,index2) in item.list" :key="item2.id">
                    <div class="condition" @click="showRight('condition',item2,index2)">
                      <div class="arrow">
                        <div class="line primary-bg" :style="{height:index2>0?'195px':'90px'} "></div>
                        <div class="triangle primary-border"></div>
                      </div>
                      <div class="info" v-show="item.condition">{{item.condition}}</div>
                    </div>
                    <div class="block-item">
                      <div class="title">
                        <div @click="showRight('checkNode',item2,index2)">
                          <i class="el-icon-edit"></i>
                          <span>{{item2.name}}</span>
                        </div>
                        <i class="el-icon-more" @mouseenter="changeShow(item2)"></i>
                      </div>
                      <!-- crud按钮 -->
                      <div
                        class="btnCotent"
                        :class="(showNode&&showNodeId==item2.id)?'showNode':'hideNode'"
                        @mouseleave="showNode=false"
                      >
                        <div @click="addNode(item2)">新增节点</div>
                        <div @click="copyNode(item2)">复制节点</div>
                        <div @click="upOrDown(item2)">上移</div>
                        <div @click="upOrDown(item2)">下移</div>
                        <div @click="delNode(item2)">删除节点</div>
                        <!-- 右边的上下方节点 -->
                        <div class="btnCotent">
                          <div @click="addNode(item2)">上方节点</div>
                          <div @click="addNode(item2)">下方节点</div>
                        </div>
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
                  <div class="condition" @click="showRight('condition',item,index)">
                    <div class="arrow">
                      <div class="line" :class="index>1?'primary-bg':'muted-bg'"></div>
                      <div class="triangle" :class="index>1?'primary-border':'muted-border'"></div>
                    </div>
                    <div class="info" v-if="index>1" v-show="item.condition">{{item.condition}}</div>
                  </div>
                  <div class="block-item">
                    <div class="title">
                      <div style="flex:1;" @click="showRight('checkNode',item,index)">
                        <i class="el-icon-edit"></i>
                        <span>{{item.name}}</span>
                      </div>
                      <i class="el-icon-more" @mouseenter="changeShow(item)"></i>
                    </div>
                    <!-- crud按钮 -->
                    <div
                      class="btnCotent"
                      :class="(showNode&&showNodeId==item.id)?'showNode':'hideNode'"
                      @mouseleave="showNode=false"
                    >
                      <div @click="addNode(item)">新增节点</div>
                      <div @click="copyNode(item)">复制节点</div>
                      <div @click="addBrach(item)">新增分支</div>
                      <div @click="upOrDown(item)">上移</div>
                      <div @click="upOrDown(item)">下移</div>
                      <div @click="delNode(item)">删除节点</div>
                      <!-- 右边的上下方节点 -->
                      <div class="btnCotent">
                        <div @click="addNode(item)">上方节点</div>
                        <div @click="addNode(item)">下方节点</div>
                      </div>
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
                      v-for="(item1,index1) in item.children"
                      :key="item1.id"
                      :style="{right: item1.num*(-290)+'px',zIndex:80-index1}"
                    >
                      <div
                        class="turnCorner"
                        :style="{width:item1.num*290+175+'px',marginLeft:-item1.num*290+'px'}"
                      ></div>
                      <div class="list-item" v-for="(item2,index2) in item1.list" :key="item2.id">
                        <div class="condition" @click="showRight('condition',item2,index2)">
                          <div class="arrow">
                            <div
                              class="line primary-bg"
                              :style="{height:index2>0?'120px':'195px'} "
                            ></div>
                            <div class="triangle primary-border"></div>
                          </div>
                          <div class="info">{{item1.condition}}</div>
                        </div>
                        <div class="block-item">
                          <div class="title">
                            <div @click="showRight('checkNode',item2,index2)">
                              <i class="el-icon-edit"></i>
                              <span>{{item2.name}}</span>
                            </div>
                            <i class="el-icon-more" @mouseenter="changeShow(item2)"></i>
                          </div>
                          <!-- crud按钮 -->
                          <div
                            class="btnCotent"
                            :class="(showNode&&showNodeId==item2.id)?'showNode':'hideNode'"
                            @mouseleave="showNode=false"
                          >
                            <div @click="addNode(item2)">新增节点</div>
                            <div @click="copyNode(item2)">复制节点</div>
                            <!-- <div @click="addBrach(item2)">新增分支</div> -->
                            <div @click="upOrDown(item2)">上移</div>
                            <div @click="upOrDown(item2)">下移</div>
                            <div @click="delNode(item2)">删除节点</div>
                            <!-- 右边的上下方节点 -->
                            <div class="btnCotent">
                              <div @click="addNode(item2)">上方节点</div>
                              <div @click="addNode(item2)">下方节点</div>
                            </div>
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
          <!-- 右边 -->
          <div class="treeRight">
            <h3 style="font-size: 16px;">节点属性</h3>
            <!-- 节点的 -->
            <div v-if="rightShowType=='startNode'||rightShowType=='checkNode'">
              <div class="formItem">
                <span>节点名称</span>
                <el-input placeholder="发起节点" v-model="choseItemData.GAI1" style="flex:1"></el-input>
              </div>
              <div class="alternative">
                <div :class="alternative=='basic'?'chose':'muted'" @click="alternative='basic'">基本属性</div>
                <div :class="alternative=='other'?'chose':'muted'" @click="alternative='other'">其他属性</div>
              </div>
              <!-- 基本属性 -->
              <div v-show="alternative=='basic'">
                <div class="copyPerson" v-if="rightShowType=='checkNode'">
                  <p style="color: #666666;margin-bottom: 7px;">负责人</p>
                  <div @click="showChosePerson('负责人')">选择负责人</div>
                </div>
                <div class="copyPerson">
                  <el-checkbox v-model="choseItemData.GAI1">启用后提交抄送</el-checkbox>
                  <div @click="showChosePerson('抄送人')">选择抄送人</div>
                </div>
                <div class="formItem">
                  <span>字段权限</span>
                  <el-input placeholder="请输入字段名称搜索" v-model="choseItemData.GAI1" style="flex:1"></el-input>
                </div>
                <div class="tableContent">
                  <div class="title">
                    <div class="first"></div>
                    <el-checkbox v-model="choseItemData.checked" style="width: 78px;">可见</el-checkbox>
                    <el-checkbox v-model="choseItemData.checked" style="width: 78px;">可编辑</el-checkbox>
                  </div>
                  <div v-for="index in 30" :key="index">
                    <div class="first">费用分类费用分类费用分类费用分类</div>
                    <el-checkbox v-model="choseItemData.checked" style="width: 78px;"></el-checkbox>
                    <el-checkbox v-model="choseItemData.checked" style="width: 78px;"></el-checkbox>
                  </div>
                </div>
              </div>
              <!-- 其他属性 -->
              <div v-show="alternative=='other'">
                <h3 style="margin-top: 20px;">节点操作</h3>
                <!-- 同意 -->
                <myCollapse>
                  <template slot="title">
                    <div class="collapse-title">
                      <span>同意</span>
                      <span class="right">已开启</span>
                    </div>
                  </template>

                  <div>
                    <form-common
                      formType="confirm"
                      :formData="formRight"
                      :formItme="confirmOptios"
                      ref="confirmForm"
                      @chnageGAIIII="chnageGAIIII"
                      :showSave="true"
                      @submitTop="saveAgree"
                    ></form-common>
                  </div>
                </myCollapse>
                <!-- 保存 -->
                <myCollapse :canChange="false">
                  <template slot="title">
                    <div class="collapse-title">
                      <span>保存</span>
                      <span class="right">已开启</span>
                    </div>
                  </template>
                </myCollapse>
                <div v-if="rightShowType=='checkNode'">
                  <!-- 驳回 -->
                  <myCollapse>
                    <template slot="title">
                      <div class="collapse-title">
                        <span>驳回</span>
                        <!-- class="right" -->
                        <span>未开启</span>
                      </div>
                    </template>
                    <div>
                      <div style="text-align: right;">
                        <mySwitch></mySwitch>
                      </div>
                      <form-common
                        formType="confirm"
                        :formData="formRight"
                        :formItme="confirmOptios"
                        ref="confirmForm"
                        @chnageGAIIII="chnageGAIIII"
                        :showSave="true"
                        @submitTop="saveAgree"
                      ></form-common>
                    </div>
                  </myCollapse>
                  <!-- 转交 -->
                  <myCollapse>
                    <template slot="title">
                      <div class="collapse-title">
                        <span>转交</span>
                        <!-- class="right" -->
                        <span>未开启</span>
                      </div>
                    </template>
                    <div>
                      <div style="text-align: right;">
                        <mySwitch :modelValue="1"></mySwitch>
                      </div>
                      <el-button
                        style="margin:0 auto;display: block;"
                        type="primary"
                        size="small"
                      >保存</el-button>
                    </div>
                  </myCollapse>
                  <!-- 流转 -->
                  <myCollapse>
                    <template slot="title">
                      <div class="collapse-title">
                        <span>流转</span>
                        <!-- class="right" -->
                        <span>未开启</span>
                      </div>
                    </template>
                    <div>
                      <div style="text-align: right;">
                        <mySwitch></mySwitch>
                      </div>
                      <form-common
                        formType="confirm"
                        :formData="formRight"
                        :formItme="confirmOptios"
                        ref="confirmForm"
                        @chnageGAIIII="chnageGAIIII"
                        :showSave="true"
                        @submitTop="saveAgree"
                      ></form-common>
                    </div>
                  </myCollapse>
                </div>
                <h3 style="margin-top: 20px;">审批意见</h3>
                <div class="spaceBetween">
                  <span>校验</span>
                  <el-checkbox v-model="choseItemData.GAIYYYY">备选项</el-checkbox>
                </div>
                <template v-if="rightShowType=='checkNode'">
                  <div class="spaceBetweenNew">
                    <span>允许流程发起人撤回</span>
                    <mySwitch></mySwitch>
                  </div>
                  <div class="spaceBetweenNew">
                    <span>允许流程发起人催办</span>
                    <mySwitch></mySwitch>
                  </div>
                  <div class="spaceBetweenNew">
                    <span>允许短信提醒</span>
                    <mySwitch></mySwitch>
                  </div>
                  <h3>流转规则</h3>
                  <el-select
                    placeholder="任意一个负责人同意后进入下一个节点"
                    v-model="choseItemData.GAI1"
                    style="width:100%; height: 57px;"
                  >
                    <el-option
                      v-for="item in ruleOptions"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                    ></el-option>
                  </el-select>
                  <h3>负责人为空时流转规则</h3>
                  <el-radio-group
                    v-model="choseItemData.GAISSS"
                    style=" height: 40px;line-height: 40px;"
                  >
                    <el-radio :label="3">跳过该节点，流转到下个节点</el-radio>
                    <el-radio :label="6">停留在该节点</el-radio>
                  </el-radio-group>
                  <h3>支出凭证生成时间</h3>
                  <el-checkbox v-model="choseItemData.GAIYYYY">本节点审批通过后自动生成</el-checkbox>
                  <P style="color:#ff6c77;font-size: 14px;">
                    *请谨慎勾选，—般为会计审核节点勾选，整个流程只
                    能有一个节点勾选
                  </P>
                </template>
              </div>
            </div>
            <!-- 条件的 -->
            <div class="rightCondition" v-if="rightShowType=='condition'">
              <div class="formItem">
                <span style="width: 100px;">数据流转条件</span>
                <el-select
                  v-model="choseItemData.GAI3333"
                  style="width: 78%;"
                  @change="changeGAI3333"
                >
                  <el-option key="GAI2" label="无条件流转" value="GAI2"></el-option>
                  <el-option key="GAI1" label="使用自定义流转条件" value="GAI1"></el-option>
                </el-select>
              </div>
              <!-- 根据上面选中的条件改变 -->
              <p v-if="false">当不定义条件或不满足自定义流转条件时，进入这条连接线下的节点</p>
              <div v-else>
                <el-button
                  type="text"
                  icon="el-icon-plus"
                  size="medium"
                  @click="showConditionAdd"
                >添加流转条件</el-button>
                <div style="color: #666666;">
                  满足以下
                  <el-select
                    v-model="choseItemData.GAI3333dffdgf"
                    style="width: 340px;margin: 0 5px;"
                    @change="changeGAI3333dffdgf"
                  >
                    <el-option key="GAI2" label="所有条件" value="GAI2"></el-option>
                    <el-option key="GAI1" label="任一条件" value="GAI1"></el-option>
                  </el-select>
                  <span>触发流转</span>
                </div>
                <!-- 条件循环 -->
                <div class="conditionItem" v-for="item in 5" :key="item">
                  <span>申请人</span>
                  <p style="width:100px">自定义范围</p>
                  <p style="width:70px">字符串</p>
                  <p style="width:200px">2022-02-18至2022-02-18</p>
                  <el-button type="text" style="color:#ff6c77;padding:0" size="medium">删除</el-button>
                </div>
                <div>
                  <h3>条件说明</h3>
                  <el-input
                    type="textarea"
                    placeholder="最多可输入50个字"
                    v-model="choseItemData.GAI9999"
                    maxlength="50"
                    show-word-limit
                  ></el-input>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- 选择抄送人/负责人 -->
      <chosePerson ref="chosePerson"></chosePerson>
      <!-- 添加流转条件 -->
      <addCondition ref="addCondition"></addCondition>
    </drawer-common>
    <dialogCommon
      :title="'添加'+addTitle"
      popupWidth="600px"
      :visible="addVisible"
      @save="addComfirm"
      @Cancel="addVisible=false"
      @handleClose="addVisible=false"
    >
      <div style="text-align: center;">
        <el-input
        style="width:300px"
          :placeholder="'请输入'+addTitle"
          v-model="addName"
        ></el-input>
      </div>
    </dialogCommon>
  </div>
</template>

<script>
import {
 getProcessNodeTreeBySettingId
} from "@/api/systemmanage/flowchat";
import drawerCommon from "@/components/common/drawerCommon.vue";
import myCollapse from "@/components/common/myCollapse.vue";
import formCommon from "@/components/common/formCommon.vue";
import mySwitch from "@/components/common/mySwitch.vue";
import chosePerson from "./chosePerson.vue";
import addCondition from "./addCondition.vue";
import dialogCommon from "@/components/common/dialogCommon.vue";
export default {
  name: "treeComponent",
  components: {
    drawerCommon,
    myCollapse,
    formCommon,
    mySwitch,
    chosePerson,
    addCondition,
    dialogCommon
  },
  data() {
    return {
      outObj:{},
      addVisible:false,
      addName:'',//添加的节点名称
      addTitle: "节点", //分支
      rightShowType: "startNode", //'startNode'  'checkNode' 'condition'
      ruleOptions: [{}],
      // checked: false,
      formRight: {},
      confirmOptios: [
        {
          label: "提交至",
          prop: "chnageGAIIII",
          confirm: true,
          type: "Select",
          options: [
            {
              id: 1,
              name: "按下个 节点的流转规则执行"
            },
            {
              id: 0,
              name: "选择下个节点负责人"
            }
          ],
          placeholder: "请选择"
        }
        // {
        //   label: "  ",
        //   prop: "chnageGAIIII222",
        //   confirm: false,
        //   type: "Select",
        //   options: [],
        //   placeholder: "请选择"
        // }
      ],
      tableData: [{}],
      alternative: "basic", //other
      choseItemData: {
        GAI3333: "GAI2"
      },
      showNode: false,
      showNodeId: null,
      flowTitle: "新增流程-设置流程",
      flowVisible: false,
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
          id: 7,
          content:
            " 负责人：信息数据部李小小、开发工程师张大一对符合法规发环境的附加费点金计划",
          children: [
            // 右边第一个分支
            {
              num: 2,
              list: [
                {
                  name: "分支11",
                  id: 8,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                },
                {
                  name: "分支12",
                  id: 9,
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
                  id: 10,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                },
                {
                  name: "分支22",
                  id: 11,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                }
              ]
            }
          ]
        },
        {
          name: "审批节点2",
          id: 12,
          condition: "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个",
          children: [
            // 右边第一个分支
            {
              num: 0,
              list: [
                {
                  name: "分支31",
                  id: 13,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                },
                {
                  name: "分支32",
                  id: 14,
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
                  id: 15,
                  condition:
                    "申请人等于张三，且金额大于1000电饭锅电饭锅发的广泛地个"
                },
                {
                  name: "分支42",
                  id: 16,
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
  methods: {
    // 显示右边
    showRight(rightType, item, index) {
      this.rightShowType = rightType;
    },
    // 设置流程
    showFlow(value) {
      this.outObj={...value}
      this.getFlowDetail()
      this.flowVisible = true;
    },
    // 根据ID查询流程
    async getFlowDetail(){
      let {data}=await getProcessNodeTreeBySettingId(this.outObj.id)
      console.log('data',data);
      
    },
    // 设置流程
    flowComfirm() {},
    // 点击框显隐
    changeShow(item) {
      this.showNode = true;
      this.showNodeId = item.id;
    },
    // 新增节点
    addNode(item) {
      this.addVisible=true
    },
    // 新增分支
    addBrach(item) {},
    // 复制节点
    copyNode() {},
    // 上下移节点
    upOrDown() {},
    // 删除节点
    delNode() {},
    // 更改其他属性提交至
    chnageGAIIII() {
      this.confirmOptios[1].confirm = true;
    },
    // 更改其他属性同意的保存
    saveAgree() {},
    //
    showChosePerson(titleType) {
      this.$refs.chosePerson.showChosePerson(titleType);
    },
    // 选中条件的时候
    // 数据流转条件
    changeGAI3333() {
      console.log("choseItemData.GAI3333", this.choseItemData.GAI3333);
    },
    // 添加流转条件显示
    showConditionAdd() {
      this.$refs.addCondition.showDia();
    },
    // 更改满足条件
    changeGAI3333dffdgf() {},
    // 添加节点、分支确定
    addComfirm(){
      this.addVisible=false
    },
  }
};
</script>
<style lang='scss' scoped>
div {
  box-sizing: border-box;
}
.bigTree {
  display: flex;
  justify-content: space-between;
  height: 100%;
  width: 100%;
  margin-top: -32px;
}
.outContainer {
  position: relative;
  height: 90vh;
  // width: 50%;
  flex: 1;
  overflow: auto;
  margin: 15px auto 0px;
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
  z-index: 2;
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
    > div {
      flex: 1;
    }
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
  z-index: 28;
  position: relative;
  .condition {
    position: relative;
    z-index: 3;
  }
  .info {
    width: 240px;
    height: 32px;
    background-color: #ffffff;
    border-radius: 4px;
    border: solid 1px #70c3fd;
    position: absolute;
    z-index: 4;
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

.treeRight {
  width: 540px;
  height: 92vh;
  overflow: auto;
  border-left: 1px solid #dddddd;
}
// 标题头
.breadcrumb {
  display: flex;
  // width: 460px;
  font-size: 14px;
  > div {
    padding: 0 5px;
  }
  p {
    display: inline-block;
    margin: 0;
    width: 48px;
    height: 20px;
    line-height: 20px;
    text-align: center;
    background-color: #eeeeee;
    border-radius: 4px;
    color: #999999;
    font-size: 12px;
    padding: 0;
  }
  img {
    width: 24px;
    vertical-align: middle;
  }
  span {
    font-size: 14px;
    color: #ff6c77;
  }
  // .line::before {
  //   display: inline-block;
  //   content: "";
  //   width: 1px;
  //   height: 12px;

  //   background-color: #dddddd;
  //   margin-top: 2px;
  // }
}

.btnCotent {
  width: 150px;
  padding: 10px;
  border-radius: 4px;
  border: 1px solid #dddddd;
  position: absolute;
  z-index: 99;

  right: 0;
  top: 0;
  background-color: #fff;
  transform: translateX(100%);
  > div {
    // height: 26px;
    line-height: 26px;
    color: #409eff;
  }
}
.showNode {
  display: block;
}
.hideNode {
  display: none;
}

// 右边
.treeRight {
  padding: 15px 25px;
  h3 {
    font-family: SourceHanSansCN-Medium;
    font-size: 14px;
    line-height: 36px;
    color: #444444;
    margin: 0px;
  }
  .formItem {
    display: flex;
    margin-top: 20px;
    span {
      display: inline-block;
      width: 66px;
      color: #666666;
      line-height: 32px;
      margin-right: 10px;
    }
  }
  .alternative {
    width: 100%;
    height: 32px;
    border-radius: 4px;
    border: 1px solid #ff6c77;
    display: flex;
    align-items: center;
    margin-top: 20px;
    > div {
      flex: 1;
      text-align: center;
      height: 100%;
      line-height: 32px;
    }
    .chose {
      background-color: #ff6c77;
      color: #ffffff;
    }
    .muted {
      color: #ff6c77;
      background-color: #fff;
    }
  }
  // 抄送人
  .copyPerson {
    margin: 20px 0px;
    > div {
      width: 100%;
      height: 50px;
      line-height: 50px;
      border-radius: 4px;
      border: 1px solid #ff6c77;
      text-align: center;
      color: #ff6c77;
      margin-top: 3px;
    }
  }
}

// 表格
.tableContent {
  border: solid 1px #dddddd;
  border-radius: 4px;
  color: #444444;
  width: 100%;
  margin-top: 15px;
  font-size: 14px;

  > div {
    height: 49px;
    line-height: 49px;
    display: flex;
    border-bottom: solid 1px #dddddd;
    padding-right: 15px;
  }
  .title {
    background-color: #fafafa;
  }
  .first {
    flex: 1;
    padding-left: 20px;
    color: #666666;
  }
}

// 其他属性
.collapse-title {
  display: flex;
  justify-content: space-between;
  line-height: 40px;
  .right {
    color: #70c3fd;
  }
}
.spaceBetween {
  display: flex;
  justify-content: space-between;
  padding: 10px 25px;
  font-family: SourceHanSansCN-Regular;
  font-size: 14px;
  line-height: 36px;
  letter-spacing: 0px;
  color: #666666;
}
.spaceBetweenNew {
  display: flex;
  justify-content: space-between;
  font-family: SourceHanSansCN-Regular;
  font-size: 14px;
  color: #444444;
  line-height: 41px;
  font-weight: 600;
}
.line-top {
  border-top: 1px solid #dddddd;
}
// 右边的条件
.rightCondition {
  .conditionItem {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
    span {
      display: inline-block;
      width: 67px;
    }
    p {
      margin: 0;
      height: 31px;
      // line-height: 32px;
      padding: 5px 10px;
      box-sizing: border-box;
      overflow: hidden;
      background-color: #dddddd;
      border-radius: 4px;
    }
  }
}
</style>