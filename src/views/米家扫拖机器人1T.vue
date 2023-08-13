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
      <div class="item29-bg">
        <h2>{{product.name}}</h2>
        <h3>{{product.subtitle}}</h3>
        <div class="price">
          <span>￥<em>{{product.price}}</em></span>
        </div>
      </div>
      <div class="item29-bg-2"></div>
      <div class="item29-bg-3"></div>
      <div class="item29-bg-4"></div>
      <div class="item29-bg-5"></div>
      <div class="item29-bg-6">
        <h2>激光导航，全屋扫描<br/>无惧复杂环境，智慧规划路线</h2>
        <br/><br/>
        <p>采用行业前沿激光导航技术，360° 扫描室内环境，迅速构建家庭户型，无惧光线和环境干扰，精准规划清扫路径。<br/>可在米家APP 一键生成 3D 户型图，立体成像清晰了然。</p>
      </div>
      <div class="item29-bg-7">
        <h2>超声波地毯识别，智能回洗<br/>自动抬升拖布，避免二次污染</h2>
        <br/><br/>
        <p>超声波识别地毯，自动抬升拖布，并开启超强挡吸力，不打湿地毯同时强力吸除灰尘。<br/>出门在外也能远程指挥，清扫客厅、卧室或厨房地面，只需米家APP 轻轻一点，即刻开始吸尘拖地。</p>
      </div>
      <div class="item29-bg-8">
        <h2>米家APP 智能联动<br/>远程控制，一键清扫</h2>
        <br/><br/>
        <p>连接米家APP，可与门锁、空净等小米智能设备联动，定制专属清洁方案，开启全屋智能清扫新体验。<br/>回洗途中，拖布也会自动抬升，避免地面二次污染。</p>
      </div>
      <div class="item29-swiper">
        <!--        轮播图四个-->
        <swiper :options="swiperOption">
          <swiper-slide><img src="./../assets/imgs/L-5.jpg"></swiper-slide>
          <swiper-slide><img src="./../assets/imgs/L-6.jpg"></swiper-slide>
          <swiper-slide><img src="./../assets/imgs/L-7.jpg"></swiper-slide>
          <swiper-slide><img src="./../assets/imgs/L-8.jpg"></swiper-slide>

          <!-- Optional controls -->
          <div class="swiper29-pagination" slot="pagination"></div>
        </swiper>
        <p class="desc"><br/>解放双手    去热爱</p>
      </div>

    </div>
  </div>
</template>
<script>
  import { swiper, swiperSlide } from 'vue-awesome-swiper'
  import ProductParam from './../components/ProductParam'
  import Modal from "../components/Modal";
  export default {
    name: '米家互联网开门冰箱',
    components: {
      Modal,
      swiper,
      swiperSlide,
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
          productId: 29,
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
            'id':29,
          }}).then((res) => {
          this.product = res.data
        })
      },
      buy () {
        const id = this.$route.params.id
        this.$router.push(`/detail/${id}`)
      },
      closeVideo () {
        // 用这个方法是解决：v-show组件显示与动画不能执行完
        // 这里利用定时器，也设置成了0.6s完成slideUp上拉动画后，再将变量showSlide置空，从而隐藏组件
        this.showSlide = 'slideUp'
        setTimeout(() => {
          this.showSlide = ''
        }, 600)
      }
    }

  }
</script>
<style lang="scss">
  @import './../assets/scss/mixin.scss';
  .product{
    .content{
      .item29-bg{
        background:url('./../assets/imgs/29-1.jpg') no-repeat center;
        height:1000px;
        padding-left: 150px;
        h2{
          font-size:80px;
          padding-top:350px;
          color: #d7d7d7;
        }
        h3{
          font-size:24px;
          letter-spacing: 10px;
          color:#333333;
        }
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
        .price{
          font-size:30px;
          color:#333333;
          em{
            font-style:normal;
            font-size:38px;
          }
        }
      }
      .item29-bg-2{
        background:url('./../assets/imgs/29-2.png') no-repeat center;
        height:480px;
        background-size:1250px 500px;
      }
      .item29-bg-3{
        background:url('./../assets/imgs/29-3.png') no-repeat center;
        height:670px;
        background-size:cover;
      }
      .item29-bg-4{
        background:url('./../assets/imgs/29-4.png') no-repeat center;
        height:670px;
        background-size:cover;
      }
      .item29-bg-5{
        background:url('./../assets/imgs/29-5.png') no-repeat center;
        height:690px;
        background-size:cover;
      }
      .item29-bg-6{
        background:url('./../assets/imgs/29-6.jpg') no-repeat center;
        height:750px;
        background-size:cover;
        text-align: center;
        h2{
          font-size:33px;
          padding-top:50px;
          color: #404040;
        }
        p {
          font-size: 15px;
          letter-spacing: 10px;
          color: #404040;
        }
      }
      .item29-bg-7{
        background:url('./../assets/imgs/29-7.jpg') no-repeat center;
        height:750px;
        background-size:cover;
        padding-left: 150px;
        h2{
          font-size:33px;
          padding-top:90px;
          color: #31180e;
        }
        p {
          font-size: 15px;
          letter-spacing: 10px;
          color: #31180e;
        }
      }
      .item29-bg-8{
        background:url('./../assets/imgs/29-8.jpg') no-repeat center;
        height:750px;
        background-size:cover;
        padding-left: 150px;
        h2{
          font-size:33px;
          padding-top:130px;
          color: #ffefd5;
        }
        p {
          font-size: 15px;
          letter-spacing: 10px;
          color: #ffefd5;
        }
      }
      .item29-swiper{
        margin:36px 30px 52px;
        .desc{
          font-size:18px;
          color:#333333;
          text-align:center;
        }
        img{
          width:100%;
        }
      }
    }
    button{
      margin-left:10px;
    }
  }
</style>
