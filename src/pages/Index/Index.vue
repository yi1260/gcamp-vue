<template>
  <div>
    <header class="m-header is-fixed is-bg top-search">
      <div class="search-wrap">
        <img src="../../assets/images/ic_search_gray.png" alt="">
        <span class="placeholder">{{randomNumber}}</span>
        <img src="../../assets/images/ic_scan_gray.png" alt="">
      </div>
      <div class="m-header-button is-right" style="text-align: center;width: 50px;">
        <a href="javascript:;"><img class="m-icon-img" src="../../assets/images/ic_chat_white.png"/></a>
      </div>
    </header>
    <div class="page-content">
      <m-swipe swipeid="swipe01" :autoplay="1000" paginationDirection="right">
        <div class="swiper-slide " slot="swiper-con"><img src="../../assets/images/banner/01.jpg" alt=""></div>
        <div class="swiper-slide " slot="swiper-con"><img src="../../assets/images/banner/02.jpg" alt=""></div>
        <div class="swiper-slide " slot="swiper-con"><img src="../../assets/images/banner/03.jpg" alt=""></div>
      </m-swipe>

      <!--<m-cell title="提醒" icon>-->
      <!--<img src="../../assets/images/ic_mine_notification.png" slot="icon">-->
      <!--<a href="javascript:;" slot="cell-right"><img src="../../assets/images/ic_arrow_gray_small.png" alt=""></a>-->
      <!--</m-cell>-->

      <!--<m-cell title="设置">-->
      <!--<a href="javascript:;" slot="cell-right"><img src="../../assets/images/ic_arrow_gray_small.png" alt=""></a>-->
      <!--</m-cell>-->

      <!--热门-->
      <div class="hot-wrap">
        <m-cell title="热门" label="hot">
          <a href="javascript:;" slot="cell-right">更多<img src="../../assets/images/ic_arrow_gray_small.png" alt=""></a>
        </m-cell>
        <m-cell-media :upvote="item.upvote" :column="item.item_id" :img="item.image_map.image0"
                      v-for="(item,index) in goodsDefaultData"
                      :key="item.item_id">
          <span slot="title">{{item.title}}</span>
          <span slot="describe">{{item.describe}}</span>
        </m-cell-media>
      </div>

      <!--推荐-->
      <!--<div class="recommend-wrap">
				<m-cell title="推荐" label="recommend"></m-cell>
				<m-cell-media  :author="item.target.author.name" :column="item.source_cn" :bg="item.target.cover_url" v-for="(item,index) in recommendData" :key="item.id">
					<span slot="title">{{item.title}}</span>
					<span slot="describe">{{item.target.desc}}</span>
				</m-cell-media>
			</div>-->
    </div>
  </div>
</template>

<script>
  import mHeader from '../../components/header'
  import mSwipe from '../../components/swipe'
  import mCell from '../../components/cell'
  import mCellMedia from '../../components/cell-media'

  export default {
    name: 'index',
    components: {
      mHeader,
      mSwipe,
      mCell,
      mCellMedia
    },
    data() {
      return {
        recommendData: [],
        hotData: [],
        randomNumber: 0,
        goodsDefaultData: [],
      }
    },

    created() {
      this.getRandomFromBackend();
      this.getGoodsDefault();
    },
    methods: {
      getRandomFromBackend () {
        const path = 'http://39.106.28.185:8080/api/test'
        this.axios.get(path)
          .then(response => {
            this.randomNumber = response.data.randomNumber
          })
          .catch(error => {
            console.log(error)
          })
      },

      getGoodsDefault() {
        const path = 'http://39.106.28.185:8080/api/goods/default'
        this.axios.get(path).then(response => {
          let data = response.data.goods;
          for (var i in data){
            this.goodsDefaultData.push(data[i])
          }
        })
      },

      fetchData() {
        this.axios.get('/api/homeData').then((response) => {
          let data = response.data.data.recommend_feeds;
          let recommend = [];
          let hot = [];
          for (var i in data) {
            if (data[i].card && data[i].card.name == '为你推荐') {
              recommend.push(data[i]);
            } else {
              hot.push(data[i]);
            }
          }

          this.recommendData = recommend;
          this.hotData = hot;

        })
      }
    }
  }

</script>

<style lang="less">
  header.m-header {
    padding: 0 0 0 10px;
  }

  .is-fixed ~ .page-content {
    padding-top: 44px;
    padding-bottom: 50px;
  }

  .top-search {
    .search-wrap {
      width: 100%;
      height: 30px;
      background: #fff;
      border-radius: 4px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      color: #c0c0c0;
      padding: 0 12px;
      .placeholder {
        flex: 1;
        text-align: left;
        padding-left: 12px;
      }
      img {
        width: 20px;
        height: 20px;
      }
    }
  }

  .hot-wrap,
  .recommend-wrap {
    padding-top: 12px;
  }

</style>

  
