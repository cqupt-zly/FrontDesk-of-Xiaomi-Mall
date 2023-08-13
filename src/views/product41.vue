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
      <div class="item41-bg">
      </div>
      <div class="item41-bg-2"></div>
      <div class="item41-bg-3"></div>
      <div class="item41-bg-4"></div>
      <div class="item41-bg-5"></div>
      <div class="item41-bg-6"></div>
      </div>
    </div>
</template>
<script>
import { swiper, swiperSlide } from 'vue-awesome-swiper'
import ProductParam from './../components/ProductParam'
import Modal from "../components/Modal";
export default {
  name: '41',
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
        productId: 41,
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
          'id':41,
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
      .item41-bg{
        background:url('https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/af962e47a4f8512263f082603aa299c0.jpg') no-repeat center;
        height:1249px;
        background-size:1226px 1249px;
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
      .item41-bg-2{
        background:url('https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/f85567727f5ce970a6ef5bcbac7c67cc.jpg') no-repeat center;
        height:1089px;
        background-size:1226px 1089px;
      }
      .item41-bg-3{
        background:url('https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/b9975b08295d9287e2a0c61669fa060f.jpg') no-repeat center;
        height:1072px;
        background-size:1226px 1072px;
      }
      .item41-bg-4{
        background:url('https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/9497243742eb1104206c291de9c6122e.jpg') no-repeat center;
        height:1042px;
        background-size:1226px 1042px;
      }
      .item41-bg-5{
        background:url('https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/1a4f53920784de50d55e3b9f6e38f481.jpg') no-repeat center;
        height:1233px;
        background-size:1226px 1233px;
      }
      .item41-bg-6{
        background:url('https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/1db83857a3cdbf9b2a1651b2f66b987d.jpg') no-repeat center;
        height:1135px;
        background-size:1226px 1135px;
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
