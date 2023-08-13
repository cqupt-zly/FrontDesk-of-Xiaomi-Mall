<template>
  <div class="product">
    <product-param :title="product.name">
      <template v-slot:buy>
        <button class="btn" @click="addCart()">立即购买</button>
      </template>
    </product-param>

    <Modal
      title="提示"
      sureText="查看购物车"
      cancelText="取消"
      btnType="3"
      modalType="middle"
      :showModal="showModal"
      @submit="goToCart"
      @cancel="showModal=false">
      <template v-slot:body>
        <p>商品添加成功！</p>
      </template>
    </Modal>

    <div class="content">
      <div class="item57-bg">
      </div>
      <div class="item57-bg-2"></div>
      <div class="item57-bg-3"></div>
      <div class="item57-bg-4"></div>
      <div class="item57-bg-5"></div>
      <div class="item57-bg-6"></div>
      <div class="item57-bg-7"></div>
      <div class="item57-bg-8"></div>
    </div>
  </div>
</template>
<script>
  import { swiper, swiperSlide } from 'vue-awesome-swiper'
  import ProductParam from './../components/ProductParam'
  import Modal from "../components/Modal";
  export default {
    name: '57',
    components: {
      Modal,
      // <ProductParam> 也可写成 <product-param>
      ProductParam
    },
    data () {
      return {
        showModal: false,

        showSlide: '', // 控制动画效果
        product: {}, // 商品信息
        swiperOption: {
          autoplay: true,
          slidesPerView: 3,
          spaceBetween: 30,
          freeMode: true,
          pagination: {
            el: '.swiper-pagination',
            clickable: true
          }
        }
      }
    },
    watch: {
      "$route": 'getProductInfo'    // 要watch route , 一旦发生变化，就调用 fetchData方法
    },
    mounted () {
      this.getProductInfo()
    },
    methods: {

      addCart (id) {
        let token;
        if(sessionStorage.getItem("token")){
          token=sessionStorage.getItem("token")
        }
        else {
          token='null'
        }
        let username= sessionStorage.getItem("username")
        const iid = this.$route.params.id;
        this.axios.post('http://localhost:8080/carts/push', {
          token:token.replace(/^\"|\"$/g,''),
          productId: 57,
          username: username,
          select: true

        }).then((res) => {
          this.showModal = true
          this.$store.dispatch('saveCartCount', res.cartTotalQuantity)
        })
      },
      goToCart () {
        this.$router.push('/cart')
      },

      getProductInfo () {
        // 获取url中的参数32， 比如：http://localhost:8080/#/product/32
        const id = this.$route.params.id
        this.axios.get(`http://localhost:8080/product/getinfo`,{
          params: {
            'id':57,
          }}).then((res) => {
          this.product = res.data
        })
      },
      buy () {
        const id = this.$route.params.id
        this.$router.push(`/detail/${id}`)
      }
    }
  }
</script>
<style lang="scss">
  @import './../assets/scss/mixin.scss';
  .product{
    .content{
      .item57-bg{
        background:url('https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/fdcfa7b5a7d2a7aaad77596d6729dfcc.jpg') no-repeat center;
        height:639px;
        background-size:1226px 639px;
        text-align:center;
        p{
          margin-top:21px;
          margin-bottom:40px;
          a{
            font-size:16px;
            color:#333333;
          }
          span{
            margin:0 15px;
          }
        }
      }
      .item57-bg-2{
        background:url('https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/9ff423020697ae3bd32d19251f712c11.jpg') no-repeat center;
        height:851px;
        background-size:1226px 851px;
      }
      .item57-bg-3{
        background:url('https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/211073af70607877bb0d64ae8834e5cb.jpg') no-repeat center;
        height:785px;
        background-size:1226px 785px;
      }
      .item57-bg-4{
        background:url('https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/5b12888c4a2e3ca83992c6d5aaf565d3.jpg') no-repeat center;
        height:846px;
        background-size:1226px 846px;
      }
      .item57-bg-5{
        background:url('https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/4b7d58927387e667f5b2e4018faa75f4.jpg') no-repeat center;
        height:797px;
        background-size:1226px 797px;
      }
      .item57-bg-6{
        background:url('https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/45dde9eb478938a3bcc2f64f53623384.jpg') no-repeat center;
        height:653px;
        background-size:1226px 653px;
      }
      .item57-bg-7{
        background:url('https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/7d4f158421d9ffdf9bbf4ad3b53983df.jpg') no-repeat center;
        height:2506px;
        background-size:1226px 2506px;
      }
      .item57-bg-8{
        background:url('https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/c1487381471dd023277075f70b7cb197.jpg') no-repeat center;
        height:1049px;
        background-size:1226px 1049px;
      }
      img{
        width:100%;
      }
    }

  }
  button{
    margin-left:10px;
  }
</style>
