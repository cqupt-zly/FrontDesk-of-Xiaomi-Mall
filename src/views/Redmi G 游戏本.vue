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

    <div class="content" style="background-color: #0D0721">
      <div class="item17-bg">
        <h2>{{product.name}}</h2>
        <h3>{{product.subtitle}}</h3>
        <div class="price">
          <span>￥<em>{{product.price}}</em></span>
        </div>
      </div>
      <div class="item17-bg-2"></div>
      <div class="item17-bg-3">
        <h2>GeForce®GTX 1650 Ti 高性能显卡<br/>游戏创作双高手，敬请大显身手</h2>
        <br/><br/>
        <p>采用屡获殊荣的 NVIDIA® GeForce® GPU，新一代图灵架构，<br/>搭配4GB GDDR6独立显存，
          拥有突破性的图形性能，为游戏带来加速体验。<br/>逼真的场景渲染，充分满足视频剪辑、平面设计、
          代码编程等需求，可助你轻松完成每个大作。</p>
      </div>
      <div class="item17-bg-4">
        <h2>16.1″ 高色域电竞大屏<br/>大视野就是这么震撼</h2>
        <p>这块大屏将全面打开你的视野，三面超窄边框设计，带来更震撼的沉浸感。<br/>100%sRGB高色域，能真实捕捉游戏色彩，逼真作战体验。</p>
      </div>
      <div class="item17-bg-5">
        <h2>144Hz 电竞级超高刷新率<br/>超速流畅，超稳射击</h2>
        <br/><br/>
        <p>一秒刷新144个画面，能让你清楚地看到对手的每个细微动作，<br/>精准锁定敌人，快速完成击杀。</p>
      </div>
      <div class="item17-bg-6">
        <h2>DTS 环绕 3D 音效<br/>听敌人就在左边</h2>
        <br/><br/>
        <p>采用DTS:X Ultra音效技术，专业调校音效系统，带来3D沉浸式立体声体验，同时 支持八种声音场景设置，<br/>可呈现更震撼的低音、更响亮的高音，无论是吃鸡还是观影，都会带来身临其境的临场感</p>
      </div>
      <div class="item17-video">
        <h2>不满足一种突破，也不安于被视野束缚<br/><br/>每一帧画面都能刷新你的感官，不受速度的局限<br/><br/>也不受流行的掌控，每一种体验都为你带来娱乐无限慢慢回味每一瞬间的精彩</h2>
        <p>Redmi G 全能高手<br/>每一面都不凡</p>
        <div class="video17-bg" @click="showSlide='slideDown'"></div>
        <div class="video17-box" v-show="showSlide">
          <div class="overlay"></div>
          <div class="video17" :class="showSlide">
            <span class="icon-close" @click="closeVideo"></span>
            <video src="./../assets/video/17.mp4"></video>
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
    name: 'Redmi G 游戏本.vue',
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
          productId: 17,
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
            'id':17,
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

      .item17-bg{
        background:url('./../assets/imgs/17-1.jpg') no-repeat center;
        height:1000px;
        text-align:center;
        h2{
          font-size:80px;
          padding-top:55px;
          color:#cccccc;
        }
        h3{
          font-size:24px;
          letter-spacing: 10px;
          color:#b0b0b0;
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
          color:#b0b0b0;
          em{
            font-style:normal;
            font-size:38px;
          }
        }
      }
      .item17-bg-2{
        background:url('./../assets/imgs/17-2.png') no-repeat center;
        height:260px;
        background-size:1500px 260px;
      }
      .item17-bg-3{
        background:url('./../assets/imgs/17-3.jpg') no-repeat center;
        height:1000px;
        background-size:cover;
        padding-left: 150px;
        h2{
          font-size:37px;
          padding-top:110px;
          color:#cccccc;
        }
        p{
          font-size:17px;
          letter-spacing: 10px;
          color:#b0b0b0;
        }
      }
      .item17-bg-4{
        background:url('./../assets/imgs/17-4.jpg') no-repeat center;
        height:850px;
        background-size:cover;
        h2{
          font-size:30px;
          padding-top:100px;
          padding-left: 150px;
          color:#cccccc;
        }
        p{
          font-size:20px;
          text-align:center;
          letter-spacing: 10px;
          padding-top:600px;
          color:#b0b0b0;
        }
      }
      .item17-bg-5{
        background:url('./../assets/imgs/17-5.jpg') no-repeat center;
        height:750px;
        background-size:cover;
        text-align:center;
        h2{
          font-size:37px;
          padding-top:70px;
          color:#cccccc;
        }
        p{
          font-size:17px;
          letter-spacing: 10px;
          color:#b0b0b0;
        }
      }
      .item17-bg-6{
        background:url('./../assets/imgs/17-6.jpg') no-repeat center;
        height:750px;
        background-size:cover;
        padding-left: 150px;
        h2{
          font-size:33px;
          padding-top:50px;
          color:#cccccc;
        }
        p {
          font-size: 15px;
          letter-spacing: 10px;
          color: #b0b0b0;
        }
      }
      .item17-video{
        height:1200px;
        background:url('./../assets/imgs/17-7.jpg');
        background-size:1460px 1200px;
        color:#FFFFFF;
        text-align:center;
        h2{
          font-size:15px;
          padding-top:470px;
          margin-bottom:47px;
        }
        p{
          font-size:15px;
          margin-bottom:58px;
        }
        .video17-bg{
          background:url('./../assets/imgs/17-8.jpg') no-repeat center;
          background-size:cover;
          width:900px;
          height:350px;
          margin:70px auto 120px;
          cursor:pointer;
        }
        .video17-box{
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
          .video17{
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
