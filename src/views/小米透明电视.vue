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
      <div class="item9-bg">
        <h2>{{product.name}}</h2>
        <h3>{{product.subtitle}}</h3>
        <div class="price">
          <span>￥<em>{{product.price}}</em></span>
        </div>
      </div>
      <div class="item9-bg-2"></div>
      <div class="item9-bg-3">
        <h2>是透明电视，也是高端OLED电视。</h2>
        <p>拥有10.7亿种色彩显示、电影级P3广色域，比起传统电视毫不逊色。让未来的一切，更具真实感。</p>
      </div>
      <div class="item9-swiper">
        <!--        轮播图四个-->
        <swiper :options="swiperOption">
          <swiper-slide><img src="./../assets/imgs/L-1.png"></swiper-slide>
          <swiper-slide><img src="./../assets/imgs/L-2.png"></swiper-slide>
          <swiper-slide><img src="./../assets/imgs/L-3.png"></swiper-slide>

          <!-- Optional controls -->
          <div class="swiper9-pagination" slot="pagination"></div>
        </swiper>
        <p class="desc">全新的显示技术，打破空间的界限</p>
      </div>
      <div class="item9-video">
        <h2>光线直接穿透屏幕，一眼透见未来。</h2>
        <p>透明OLED屏，摒弃了传统背板的同时，在极其微小的像素空间内，将线路和微
          型器件通过精密排布设计，带来颠覆性的透明形态。</p>
        <div class="video9-bg" @click="showSlide='slideDown'"></div>
        <div class="video9-box" v-show="showSlide">
          <div class="overlay"></div>
          <div class="video9" :class="showSlide">
            <span class="icon-close" @click="closeVideo"></span>
            <video src="./../assets/video/9-5.mp4" muted autoplay controls="controls"></video>
          </div>
        </div>
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
          productId: 9,
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
            'id':9,
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
      .item9-bg{
        background:url('./../assets/imgs/9-1.png') no-repeat center;
        height:718px;
        text-align:center;
        h2{
          font-size:80px;
          padding-top:55px;
        }
        h3{
          font-size:24px;
          letter-spacing: 10px;
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
      .item9-bg-2{
        background:url('./../assets/imgs/9-2.png') no-repeat center;
        height:700px;
        background-size:1500px 700px;
      }
      .item9-bg-3{
        h2{
          font-size:60px;
          padding-top: 230px;
          margin-bottom:47px;

        }
        p{
          font-size:24px;
          margin-bottom:58px;
        }
        text-align:center;
        background:url('./../assets/imgs/9-3.png') no-repeat center;
        height:638px;
        background-size:cover;
      }
      .item9-swiper{
        margin:36px auto 52px;
        .desc{
          font-size:18px;
          color:#333333;
          text-align:center;
        }
        img{
          width:100%;
        }
      }
      .item9-video{
        height:1044px;
        background-color:#070708;
        color:#FFFFFF;
        text-align:center;
        h2{
          font-size:50px;
          padding-top:82px;
          margin-bottom:47px;
        }
        p{
          font-size:20px;
          margin-bottom:58px;
        }
        .video9-bg{
          background:url('./../assets/imgs/9-4.jpg') no-repeat center;
          background-size:cover;
          width:1000px;
          height:500px;
          margin:0 auto 120px;
          cursor:pointer;
        }
        .video9-box{
          .overlay{
            @include position(fixed);
            background-color:#333333;
            opacity:.4;
            z-index:10;
          }
          @keyframes slideDown{
            from{
              top:-50%;
              opacity:0;
            }
            to{
              top:50%;
              opacity:1;
            }
          }
          @keyframes slideUp{
            from{
              top:50%;
              opacity:1;
            }
            to{
              top:-50%;
              opacity:0;
            }
          }
          .video9{
            position:fixed;
            top:50%;
            left:50%;
            transform:translate(-50%,-50%);
            z-index:10;
            width:1000px;
            height:536px;
            &.slideDown{
              animation:slideDown 0.6s linear;
            }
            &.slideUp{
              animation:slideUp 0.6s linear;
            }
            .icon-close{
              position:absolute;
              top:20px;
              right:20px;
              @include bgImg(20px,20px,'https://cdn.jsdelivr.net/gh/ZTY18873242003/img/优雅的使用图床/icon-close.png');
              cursor:pointer;
              z-index:15;
            }
            video{
              width:100%;
              height:100%;
              object-fit:cover;
              outline:none;
            }
          }
        }
      }
    }
    button{
      margin-left:10px;
    }
  }
</style>

