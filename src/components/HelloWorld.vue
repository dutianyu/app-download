<template>
  <div class="content">
    <div class="main">
      <ul>
        <li
          v-for="(v,i) in list"
          :key="i"
          :class="active===i ? 'active' : null"
          @click.stop="active=i"
        >
          <span>{{v.name}}</span>
          <span>{{v.version}}</span>
          <div @click.stop="handerClickDown(v)"></div>
        </li>
      </ul>
    </div>

    <div class="footer">
      <div class="btn-box">
        <div class="text">{{activeName}}</div>
        <van-button
          round
          block
          :color="btnColor"
          :icon="require('@/assets/android.png')"
          @click.stop="androidClick"
        >下 载</van-button>
      </div>
      <div class="btn-box">
        <div class="text">版本: {{activeVersion.replace('v','')}}</div>
        <van-button
          round
          block
          :color="btnColor"
          :icon="require('@/assets/apple.png')"
          @click.stop="iosClick"
        >下 载</van-button>
      </div>
    </div>

    <van-action-sheet v-model="show" :actions="actions" :title="title" @select="onSelect" />
  </div>
</template>

<script>
import { Button, ActionSheet } from 'vant'
import 'vant/lib/button/style'
import 'vant/lib/action-sheet/style'

export default {
  name: 'HelloWorld',
  components: {
    [Button.name]: Button,
    [ActionSheet.name]: ActionSheet
  },
  data() {
    return {
      show: false,
      actions: [],
      title: null,
      btnColor: '#7384f0',
      active: 0,
      list: window.list
    }
  },
  computed: {
    activeName() {
      return this.list[this.active].name
    },
    activeVersion() {
      return this.list[this.active].version
    }
  },
  methods: {
    onSelect(item) {
      const { name, app } = item
      const findIndex = this.list.findIndex(v => v.name === app)
      const findItem = this.list.find(v => v.name === app)
      findItem.version = name
      this.active = findIndex
      this.show = false
    },
    handerClickDown(val) {
      const { historys, name } = val
      if (!Array.isArray(historys) || historys.length < 1) return false
      this.actions = historys.map(v => {
        let item = Object.assign({ app: name }, v)
        return item
      })
      this.title = name
      this.show = true
    },
    androidClick() {
      const item = this.list[this.active]
      const { version } = item
      const { android } = item.historys.find(v => v.name === version)
      window.location.href = android
    },
    iosClick() {
      const item = this.list[this.active]
      const { version } = item
      const { ios } = item.historys.find(v => v.name === version)
      window.location.href = ios
    }
  },
  beforeCreate() {
    document.title = '先腾数据技术有限公司-APP下载'
  },
  created() {
    if (Array.isArray(this.list) && this.list.length) {
      this.handerClickDown(this.list[0])
      this.show = false
    }
  }
}
</script>

<style lang="scss" scoped>
$btn-bg: #7384f0;
$font-color: #5064eb;
$cell-bg: #edeffc;

.content {
  box-sizing: border-box;
  padding-top: 221px;
  color: $font-color;
  font-size: 18px;
}

.main {
  height: calc(100vh - 221px - 100px);
  box-sizing: border-box;
  padding: 0 15px;
  ul {
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-around;
    li {
      background-color: $cell-bg;
      display: flex;
      align-items: center;
      justify-content: space-between;
      width: 100%;
      height: 54px;
      box-sizing: border-box;
      padding: 0 20px;
      border-radius: 25px;
      transition: all 0.21s linear;
      span {
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }
      span:first-of-type {
        flex: 2;
      }
      span:last-of-type {
        flex: 1;
      }
      div {
        width: 28px;
        height: 28px;
        background-image: url(~@/assets/down.png);
        background-size: 100% 100%;
      }
      div:active {
        background-image: url(~@/assets/down-white.png);
      }
    }
    li.active {
      background-color: $font-color;
      color: white;
      div {
        background-image: url(~@/assets/down-white.png);
      }
      div:active {
        background-image: url(~@/assets/down.png);
      }
    }
  }
}

.footer {
  width: 100%;
  height: 100px;
  box-sizing: border-box;
  padding: 0 15px;
  display: flex;
  align-items: center;
  justify-content: space-around;
  .btn-box {
    width: 0;
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    .text {
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      text-align: center;
      width: 100%;
      margin-bottom: 7px;
      color: grey;
    }
  }
  .btn-box:first-of-type {
    margin-right: 7.5px;
  }
  .btn-box:last-of-type {
    margin-left: 7.5px;
  }
}
</style>