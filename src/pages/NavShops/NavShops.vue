<template>
  <div class="navshops">
    <HeaderTop :title="getCategoryTitle">
      <a slot="left" class="back" @click="$router.back()">
        <i class="iconfont icon-arrow_left"></i>
      </a>
    </HeaderTop>
    <div class="navshops_list">
      <ShopList ref="shopsList"/>
    </div>
  </div>
</template>

<script>
import HeaderTop from "../../components/HeaderTop/HeaderTop.vue";
import ShopList from '../../components/ShopList/ShopList.vue'
import BScroll from 'better-scroll'

import { mapState } from "vuex";

export default {
  components: {
    HeaderTop,
    ShopList
  },
  mounted () {
    this.$store.dispatch('getCategorys')
    this.$store.dispatch('getShops', () => {
      this.$nextTick(() => {
        new BScroll('.navshops_list', {
          click: true
        })
      })
    })
  },
  computed: {
    ...mapState(["categorys"]),

    getCategoryTitle() {
      const { id } = this.$route.query;
      const { categorys } = this;
      if (id && categorys.length) {
        return categorys.filter(category => category.id === parseInt(id))[0]
          .title;
      }
    }
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixins.styl';

.navshops {
  .back {
    position: absolute;
    top: 10px;
    left: 0;

    .icon-arrow_left {
      display: block;
      padding: 5px;
      font-size: 20px;
      color: #fff;
    }
  }
  .navshops_list {
    margin-top: 50px;
  }
}
</style>
