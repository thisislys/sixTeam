<template>
  <div class="wrap">
    <header>
      <span
        :class="active===index?'active':''"
        v-for="(item, index) in types"
        :key="index"
        @click="tabChange(index)"
      >{{item}}</span>
    </header>
    <sList :list="list"></sList>
    <p class="more" v-if="list.length">{{hasMore?'上拉加载更多': '我是有底线的'}}</p>
  </div>
</template>

<script>
import { getLocation, getAuth } from "@/utils/index.js";
import sList from "../../components/slist.vue";
import { mapState, mapMutations, mapActions } from "vuex";


export default {
  data() {
    return {
      types: ["全部面试", "未面试", "已面试", "已放弃"]
    };
  },
  components: {
    sList
  },
  computed: {
    ...mapState({
      active: state => state.sign.active,
      list: state => state.sign.list,
      page: state => state.sign.page,
      hasMore: state => state.sign.hasMore
    })
  },
  methods: {
    ...mapMutations({
      updataState: "sign/updataState"
    }),
    ...mapActions({
      getList: "sign/getList"
    }),
    tabChange(index) {
      this.updataState({ active: index, page: 1 });
      this.getList();
    }
  },
  onShow() {
    this.getList();
  },
  onReachBottom() {
    console.log(this.list.length,'list')
    console.log('触发了',this.hasMore)
    let page=this.page + 1
    if (this.hasMore) {
      this.updataState({ page });
      this.getList();
    }
  }
};
</script>

<style lang="scss" scoped>
.wrap {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
}
header {
  background: #f4f6f9;
  width: 100%;
  height: 110rpx;
  line-height: 100rpx;

  display: flex;
  align-items: center;
  span {
    flex: 1;
    text-align: center;
    font-size: 34rpx;
    border-bottom: 10rpx solid #fff;
  }
  span.active {
    border-bottom: 10rpx solid rgb(55, 147, 184) !important;
  }
}
.more{
  
  position: absolute;
  bottom:-1rpx;
  left:35%;
}
// .orderInfo{
//   width: 100%;
//   height: 100%;
//   background:skyblue;
//   padding: 20rpx 20rpx;
//   box-sizing: border-box;
//   overflow-y: auto;
//   li{
//     width: 100%;
//     // height:380rpx;
//     background: #ffffff;
//     // box-shadow:5rpx 2rpx 2rpx 5rpx #cfcfcf;
//     display:flex;
//     flex-direction: column;
//     justify-content: space-between;
//     border-radius:30rpx;
//     padding: 0 20rpx;
//     margin-bottom: 20rpx;
//     box-sizing: border-box;
//     h3{
//       width: 100%;
//       height:100rpx;
//       line-height:100rpx;
//       font-size:36rpx;

//     }
//     h4{
//       width: 100%;
//       height:100rpx;
//       line-height:100rpx;
//       border-top:1rpx dashed #cfcfcf;
//       font-size:36rpx;
//     }
//     div,p{
//       width: 100%;
//       height:100rpx;
//       line-height: 100rpx;
//       display: flex;
//       button{
//         width: 200rpx;
//         height:80rpx;
//         line-height:80rpx;
//         border: 0;
//         outline: none;
//         font-size: 36rpx;
//         border: 1rpx solid skyblue;
//         border-radius:20rpx;
//       }
//       button.btnclass{
//         color:#ffffff;
//         background: skyblue
//       }
//     }
//     p{
//       justify-content: space-between;
//       span:nth-child(1){
//         font-size: 40rpx;
//       }
//       span:nth-child(2){
//         color: skyblue;
//       }
//     }
//   }
// }
</style>
