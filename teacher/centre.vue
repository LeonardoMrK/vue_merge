<template>
<div>
    <grid :show-lr-borders="false" :show-vertical-dividers="false">
      <grid-item link="/teacher-students" :label="'学员列表'">
        <img slot="icon" src="@/assets/image/35026108/图层 6.png">
      </grid-item>
      <grid-item link="/students-list" :label="'教师考勤'">
        <img slot="icon" src="@/assets/image/35026108/图层 7.png">
      </grid-item>
      <grid-item link="/students-list" :label="'我的请假'">
        <img slot="icon" src="@/assets/image/35025722/图层 21.png">
      </grid-item>
    </grid>
    <grid :show-lr-borders="false" :show-vertical-dividers="false">
      <grid-item link="/teacher-calendar" :label="'日历'">
        <img slot="icon" src="@/assets/image/35026108/日历图标3.png">
      </grid-item>
      <grid-item link="/teacher-course" :label="'课表'">
        <img slot="icon" src="@/assets/image/35026108/图层 2.png">
      </grid-item>
      <grid-item link="/teacher-homework" :label="'作业'">
        <img slot="icon" src="@/assets/image/35026108/图层 4.png">
      </grid-item>
    </grid>
    <grid :show-lr-borders="false" :show-vertical-dividers="false">
      <grid-item link="/teacher-calendar" :label="'单词'">
        <img slot="icon" src="@/assets/image/35026108/图层 9.png">
      </grid-item>
      <grid-item link="/teacher-students" :label="'成绩'">
        <img slot="icon" src="@/assets/image/35025722/图层 6.png">
      </grid-item>
      <grid-item link="/students-list" :label="'office hour' ">
        <img slot="icon" src="@/assets/image/35025722/图层 13.png">
      </grid-item>
    </grid>
    <grid :show-lr-borders="false" :show-vertical-dividers="false">
      <grid-item link="/teacher-change" :label="'改课'">
        <img slot="icon" src="@/assets/image/35026108/图层 16.png">
      </grid-item>
      <grid-item link="/students-list" :label="'绩效统计'">
        <img slot="icon" src="@/assets/image/35026108/图层 20.png">
      </grid-item>
      <grid-item link="/students-list" :label="'报表'">
        <img slot="icon" src="@/assets/image/35026108/图层 5.png">
      </grid-item>
    </grid>
</div>
</template>



<script>
  import {Grid, GridItem, GroupTitle, Group, Toast} from 'vux'
  import axios from 'axios'
  import store from '@/store'

  export default {
    components: {
      Group,
      Grid,
      GridItem,
      GroupTitle,
      Toast,
      store
    },
    data() {
      return {
        gardenId: '',
        garden_url: '',
        gardenId_wx: '',
        showPositionValue: false,
      }
    },
    created() {
      console.log(this.$route)
    },
    methods: {
      href_to(_url) {
        if (_url == '') {
          this.showPositionValue = true;
        } else {
          this.$router.push({path: '/iframe', query: {url: _url}});
        }
      }
    }
  }
</script>



<style lang="less">
  body {
    background-color: #efeff4 !important;
  }

// //学员列表
// .xylb{
//   color:rgb(255, 255, 255);//字体颜色
//   font-size:1px;
//   font-family: Arial, Helvetica, sans-serif
// }


  #application {
    margin-top: 16px;
    position: absolute;
    width: 100%;
    .itemtitle {
      display: flex;
      align-items: center;
    }

    .itemtitle:before {
      content: '';
      display: inline-block;
      position: relative;
      top: 10px;
      width: 4px;
      height: 15px;
      margin: 0px 5px 0px 16px;
      background-color: #72bff3;
    }



    .weui-grid {
      height: 80px;
      text-align: center;
      font-size: 6.5px;
      color: #000000;
    }

    .weui-grid .weui-grid__label {
      font-size: 13px;
      margin: 4px 0px 12px;
      color: rgba(0, 0, 0, 0.9);
    }
    .weui-grid:before {
      border-right: 1px solid #ffffff;
    }
    .weui-grid:after {
      border-bottom: 1px solid #ffffff;
    }
    .weui-grids:before {
      border-top: 1px solid #ffffff;
    }
    .weui-grids {
      margin: 16px 0px;
      padding-bottom: 16px;
      border-bottom: 5px solid #f6f6f6;
      a {
        width: 25% !important;
      }
    }
    .weui-grid__icon {
      img {
        height: 20px;
        width: 20px;
        margin: auto;
      }
    }
  }
</style>