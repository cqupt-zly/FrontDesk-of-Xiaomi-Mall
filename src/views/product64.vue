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
      <div class="item64-bg">
      </div>
      <div class="item64-bg-2"></div>
      <div class="item64-bg-3"></div>
      <div class="item64-bg-4"></div>
    </div>
  </div>
</template>
<script>
  import { swiper, swiperSlide } from 'vue-awesome-swiper'
  import ProductParam from './../components/ProductParam'
  import Modal from "../components/Modal";
  export default {
    name: '64',
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
          productId: 64,
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
            'id':64,
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
      .item64-bg{
        background:url('../../public/imgs/product/64.1.png') no-repeat center;
        height:863px;
        background-size:1900px 863px;
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
      .item64-bg-2{
        background:url('../../public/imgs/product/64.2.png') no-repeat center;
        height:7904px;
        background-size:1887px 7904px;
      }
      .item64-bg-3{
        background:url('../../public/imgs/product/64.3.png') no-repeat center;
        height:4417px;
        background-size:1891px 4417px;
      }
      .item64-bg-4{
        background:url('../../public/imgs/product/64.4.png') no-repeat center;
        height:1070px;
        background-size:1883px 1070px;
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
