<template>
  <div class="tab-com">
    <div class="widget">
      <div class="table-overflow" v-if="list.length > 0">
        <table class="table table-hover">
          <thead>
          <tr>
            <th :key="index" v-for="(item, index) in tabConfig" :width="item.len" :style="{ minWidth: item.min, maxWidth: item.max }">
              {{ item.value }}
            </th>
          </tr>
          </thead>
          <tbody>
          <tr :key="index" v-for="(item, index) in list">
            <td class="ck"><label><input type="checkbox" class="input-sm" v-model="item.is"></label></td>
            <td class="ck-num">{{ index + 1 }}</td>
            <td :key="index1" v-for="(obj, index1) in tabConfig"
                v-if="(obj.name !== 'checked')&&(obj.name !== 'num')&&(obj.name!=='operation')"
            >
              {{ item[obj.name] }}
            </td>
            <td class="text-center">
              <button type="button" class="btn btn-success btn-sm" @click="seeEvent(item)">查看</button>
              <button type="button" class="btn btn-danger btn-sm" @click="deleteEvent(item)">删除</button>
              <button type="button" class="btn btn-success btn-sm" @click="editEvent(item)">编辑</button>
            </td>
          </tr>
          <tr>
            <td :colspan="tabConfig.length">
              <div class="pageNumber text-center">
                <div class="checkbox">
                  <label>
                    <input type="checkbox" v-model="checked" @change="checkTogEv">全选
                  </label>
                  <button type="button" class="btn btn-danger btn-sm" @click="batchDelete">批量删除</button>
                </div>
                <div class="dataTables_paginate" id="example2_paginate">
                  <ul class="pagination pagination-sm">
                    <li class="total">总数{{ total }}条</li>
                    <li :class="search.pageNo === 1 ? 'paginate_button previous disabled' : 'paginate_button previous'" id="example2_previous" @click="previousPageEvent">
                      <a aria-controls="example2" data-dt-idx="0" tabindex="0">上一页</a>
                    </li>
                    <li :class="lastPage === search.pageNo ? 'paginate_button active' : 'paginate_button'" :key="lastPage" v-for="lastPage in last_page" @click="pageEvent(lastPage)">
                      <a aria-controls="example2" data-dt-idx="1">{{ lastPage }}</a>
                    </li>
                    <li :class="search.pageNo === last_page ? 'paginate_button next disabled' : 'paginate_button next' " id="example2_next">
                      <a aria-controls="example2" data-dt-idx="7" tabindex="0" @click="nextPageEvent">下一页</a>
                    </li>
                  </ul>
                </div>
              </div>
            </td>
          </tr>
          </tbody>
        </table>
      </div>
      <div class="table-overflow text-center" v-else>
        暂无数据，请点击页签右侧“新增”按钮新增数据
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'tab-com',
  components: {
    /* 组件存放 */
  },
  data () {
    /* 数据存放 */
    return {
    }
  },
  props: {
    search: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return {}
      }
    },
    tabConfig: {
      type: Array,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return []
      }
    },
    list: {
      type: Array,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return []
      }
    },
    total: {
      type: Number,
      default: function () {
        return 0
      }
    },
    per_page: {
      type: Number,
      default: function () {
        return 0
      }
    },
    current_page: {
      type: Number,
      default: function () {
        return 0
      }
    },
    last_page: {
      type: Number,
      default: function () {
        return 0
      }
    },
    checked: {
      type: Boolean,
      default: function () {
        return false
      }
    }
  },
  beforeRouteEnter (to, from, next) {
    /* 路由进入之前触发 */
    next()
  },
  beforeRouteUpdate (to, from, next) {
    /* 路由更新触发 */
    next()
  },
  beforeRouteLeave (to, from, next) {
    /* 路由离开之前触发 */
    next()
  },
  activated () {
    /* keep-alive组件激活时调用 */
  },
  deactivated () {
    /* keep-alive组件停用时调用 */
  },
  created () {
    /* HTML渲染之前触发 */
  },
  mounted () {
    /* DOM加载完毕触发 */
  },
  computed: {
    /* 计算属性 */
  },
  watch: {
    /* 侦听器 */
  },
  methods: {
    /* 方法集合 */
    pageEvent (pageNo) {
      /* 点击翻到多少页 */
      this.$emit('pageevent', pageNo)
    },
    previousPageEvent () {
      /* 上一页事件 */
      let _this = this
      if (_this.search.pageNo === 1 && (_this.search.pageNo > 0)) {
        return alert('已经是首页了')
      }
      _this.search.pageNo = _this.search.pageNo - 1
      this.$emit('previouspageevent', _this.search.pageNo)
    },
    nextPageEvent () {
      /* 下一页事件 */
      let _this = this
      if (_this.search.pageNo === _this.last_page && (_this.search.pageNo > 0)) {
        return alert('已经是尾页了')
      }
      _this.search.pageNo = _this.search.pageNo + 1
      this.$emit('nextpageevent', _this.search.pageNo)
    },
    seeEvent (item) {
      /* 查看 */
      this.$emit('seeevent', item)
    },
    editEvent (item) {
      /* 编辑 */
      this.$emit('editevent', item)
    },
    deleteEvent (item) {
      /* 删除 */
      this.$emit('deleteevent', item)
    },
    checkTogEv () {
      /* checked change事件 */
      let _this = this
      if (_this.checked) {
        for (let i = 0; i < _this.list.length; i++) {
          _this.list[i].is = true
        }
      } else {
        for (let i = 0; i < _this.list.length; i++) {
          _this.list[i].is = false
        }
      }
    },
    batchDelete () {
      /* 批量删除事件 */
      let _this = this
      this.$emit('batchdelete', _this.list)
    }
  },
  beforeDestroy () {
    /* 实例销毁之间调用 */
  },
  destroyed () {
    /* Vue实例销毁后调用 */
  }
}
</script>
<style lang="scss" scoped>
  .tab-com{
    background: #fafafa;
    .widget{
      padding:0px 20px;
      .table-overflow{
        .table.table-hover{
          .input-sm{
            cursor: pointer;
          }
          thead{
            border-bottom: 1px solid #D2D6DE;
            box-shadow: none;
          }
          tr,td,th{
            border: 1px solid #D2D6DE;
            vertical-align: middle;
            color: #555555;
          }
          .ck,.ck-num{
            text-align: center;
            line-height: 5px
          }
          td{
            .pageNumber{
              padding: 0px 10px 0px 10px;
              .checkbox{
                display: inline-block;
                font-size: 12px;
                vertical-align: middle;
                line-height: 20px;
                float: left;
                cursor: pointer;
                .btn{
                  margin-left: 10px;
                  cursor: pointer;
                }
                input{
                  cursor: pointer;
                }
              }
              .total{
                vertical-align: middle;
                line-height: 30px;
                padding-left: 10px;
              }
              #example2_paginate{
                .pagination{
                  margin: 10px!important;
                  li{
                    cursor: pointer;
                  }
                }
              }
            }
          }
        }
      }
      .table-overflow.text-center{
        color: #555555;
        border: 1px solid #D2D6DE;
        padding: 6px;
      }
    }
  }
</style>
