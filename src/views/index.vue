<template>
  <div class="page">
    <header class="header">
      <div class="nav-wrapper">
        <nav class="navbar navbar-expand-lg navbar-light bg-light">
          <a class="navbar-brand mr-lg-auto" href="#"><img src="images/logo.svg" alt=""></a>
          <!-- 搜尋 -->
          <div>
            <v-select :options="optionsList" label="name" v-model="selected" placeholder="search"
              style="width:200px;background-color: #fff;border-radius: 4px;">
            </v-select>
          </div>
        </nav>
      </div>
    </header>
    <div class="wrap-container">
      <section class="banner-wrap container-fluid">
        <div class="row no-gutters">
          <div class="order-lg-1 order-0 bnr-wrap col-lg-5">
            <div id="carouselExampleControls" class="banner carousel slide" data-ride="carousel">
              <div class="carousel-inner">
                <!-- banner要預設第一張active -->
                <div :class="['carousel-item', {'active': i === 0}]" v-for="(item, i) in bannerItem" :key="i">
                  <img class="d-block w-100" :src="`images/a/${item.img}`">
                </div>
              </div>
              <a class="carousel-control-prev" href="#carouselExampleControls" role="button" data-slide="prev">
                <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                <span class="sr-only">Previous</span>
              </a>
              <a class="carousel-control-next" href="#carouselExampleControls" role="button" data-slide="next">
                <span class="carousel-control-next-icon" aria-hidden="true"></span>
                <span class="sr-only">Next</span>
              </a>
            </div>
          </div>
          <div class="pulse-button d-lg-none">
            <div class="collapse-ad">展開廣告</div>
            <div class="arrow-2-x">
              <span class="iconfont iconarrowdown chevron-down"></span>
            </div>
          </div>
          <div class="order-lg-0 order-1 bnr-wrap ad col-lg display-none">
            <div class="bnr-item-wrap row no-gutters firstAd">
              <!-- b區1~6 小廣告圖 -->
              <div v-for="(item, i) in adItem" :key="i"
                :class="['bnr-item col-md-4 col-3' ,`${item.key}` === 'display' ? 'd-none d-md-block' : '']">
                <a :href="item.url" target="_blank">
                  <img :src="`images/b/${item.img}`" alt="">
                </a>
              </div>
            </div>
          </div>
          <div class="order-lg-2 order-2 bnr-wrap ad col-lg display-none">
            <div class="bnr-item-wrap row no-gutters subAd">
              <!-- b區7~12 小廣告圖 -->
              <div v-for="(item, i) in adSubItem" :key="i"
                :class="['bnr-item col-md-4 col-3', `${item.key}` === 'display' ? 'd-block d-md-none' : '' ]">
                <a :href="item.url" target="_blank">
                  <img :src="`images/b/${item.img}`" alt="">
                </a>
              </div>
            </div>
          </div>
        </div>
      </section>
      <!-- 下方內容 -->
      <section class="content-wrap container">
        <div class="row no-gutters">
          <div class="order-lg-0 order-1 col-lg-2 col-md-12 d-flex flex-column-reverse flex-lg-column">
            <div class="popular-wrap">
              <div class="popular-title">熱門推薦</div>
              <div class="popular-list">
                <!-- 熱門推薦列表 跑getPopular資料-->
                <div class="popular-item" v-for="(item, i) in getPopular" :key="i">
                  <a :href="`${item.url}`" target="_blank">
                    <div class="item-title">{{ item.name }}</div>
                    <div class="popular-member-btn">
                      <i class="iconfont iconarticle"></i>
                    </div>
                    <div v-if="selectButton==='進階護膚'" class="popular-manage-btn">
                      <a :href="`${item.urlManage}`" target="_blank">
                        <i class="iconfont iconbuy"></i>
                      </a>
                    </div>
                  </a>
                </div>
              </div>
            </div>
            <div class="e-ad-wrap ad d-none d-lg-block">
              <div class="e-ad-item">
                <a href="#" target="_blank"><img src="images/e/bigad03.png" alt=""></a>
              </div>
            </div>
          </div>
          <div class="order-lg-1 order-2 col-lg-8 col-md-12">
            <div class="tab-wrap">
              <div class="nav-wrap">
                <div class="swiper-content">
                  <template>
                    <div v-for="(item, i) in nav" :key="i" class="nav-item d-inline-block nav-lines">
                      <div class="tab" @click="selectTab(item.text)">
                        <span>{{ item.text }}</span>
                      </div>
                    </div>
                  </template>
                </div>
              </div>
              <div class="tab-item">
                <ul class="nav nav-tabs" id="myTab" role="tablist">
                  <!-- 各類別tab -->
                  <li v-for="(item, i) in getData" :key="i" @click="selectGame(item.name)"
                    class="nav-item  tabList">
                    <span :class="[{active: item.name === num}, 'nav-link']" href="">{{ item.name.replace("NUM_", "") }}</span>
                  </li>
                </ul>
                <div class="tab-content" id="myTabContent">
                  <!-- 各大系統內容 -->
                  <div class="system-content">
                    <!-- 假如不是"全部"時 全部為''-->
                    <div v-if="currentTabName != ''">
                      <div v-for="(items, i) in currentData" :key="i" class="system-list">
                        <div class="system-title">{{ items.name }}</div>
                        <div v-for="(item, i) in items.list" :key="i" class="system-item">
                          <a :href="`${item.url}`" target="_blank">
                            {{ item.name }}
                            <div v-if="selectButton ==='基礎保養'|| selectButton==='進階護膚'" class="system-member-btn">
                              <i class="iconfont iconarticle"></i>
                            </div>
                            <div v-if="selectButton ==='進階護膚'" class="system-manage-btn">
                              <a :href="`${item.urlManage}`" target="_blank">
                                <i class="iconfont iconbuy"></i>
                              </a>
                            </div>
                          </a>
                        </div>
                      </div>

                    </div>
                    <div v-else>
                      <!-- 全部 -->
                      <div v-for="(items, i) in currentAllData" :key="i" class="system-list">
                        <div v-if="items.content.length > 0">
                          <div v-for="(itemj, j) in items.content" :key="j">
                            <div class="system-title">{{ itemj.name }}</div>
                            <div v-for="(itemk, k) in itemj.list" :key="k" class="system-item">
                              <a :href="`${itemk.url}`" target="_blank">
                                <div class="item-title">{{ itemk.name }}</div>
                                <div v-if="selectButton ==='基礎保養'|| selectButton==='進階護膚'" class="system-member-btn">
                                  <i class="iconfont iconarticle"></i>
                                </div>
                                <div v-if="selectButton ==='進階護膚'" class="system-manage-btn">
                                  <a :href="`${itemk.urlManage}`" target="_blank">
                                    <i class="iconfont iconbuy"></i>
                                  </a>
                                </div>
                              </a>
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="order-lg-2 order-0 col-lg-2 col-md-12">
            <div class="e-ad-wrap ad display-none">
              <div class="e-ad-item">
                <a href="#" target="_blank"><img src="images/e/bigad01.png" alt=""></a>
              </div>
              <div class="e-ad-item">
                <a href="#" target="_blank"><img src="images/e/bigad02.png" alt=""></a>
              </div>
              <div class="e-ad-item d-block d-lg-none">
                <a href="#" target="_blank"><img src="images/e/bigad03.png" alt=""></a>
              </div>
            </div>
          </div>
        </div>
      </section>
      <footer class="footer pb-2 pt-2">
        <div class="text-center footer-text">© 2020 Beauty care experience and ranking</div>
      </footer>
      <a href="#" class="toTop" title="GoTop" style="display: inline;"><span></span></a>
    </div>
  </div>
</template>
<script>
  import $ from 'jquery'
  import "vue-select/dist/vue-select.css"
  // import imagesJson from '@/assets/json/images.json' //直接匯入json資料

  export default {
    name: "",
    props: {},
    data: function () {
      // 資料
      return {
        src: "", //追蹤 store用
        // 搜尋的選單
        optionsList: [],
        selected: '', // 搜尋選擇的項目
        // 搜尋選單的url
        searchUrl: [],
        //中間banner(@用ajax資料會有非同步問題吃不到swiper套件功能@)
        bannerItem: [],
        //banner左方1~6小廣告圖
        adItem: [],
        //banner右方7~12小廣告圖
        adSubItem: [],
        //搜尋內容
        searchItem: [],
        // 熱門推薦的內容
        getPopular: [],
        nav: [{
            'text': '基礎保養'
          },
          {
            'text': '進階護膚'
          },
          {
            'text': '防曬'
          },
          {
            'text': '底妝'
          },
          {
            'text': '彩妝'
          }
        ],
        // 裡面的內容
        getData: [],
        num: '全部',
        AllData: {},
        currentData: [],
        currentTabName: '', // 只有[全部]不會設定該值
        currentAllData: [],
        selectButton: ''
      }
    },
    watch: {
      //監聽值
      'selected'(val) {
        // if (value) window.open(this.itemsAll[value])
        // this.selected = ''
        for(let i in this.searchUrl){
          if (val === this.searchUrl[i].text) window.open(this.searchUrl[i].url)
        }
      },
      'getData'(items) {
        // 清除Tab資料
        this.clearTabData()
        if (this.selectButton === '基礎保養' || this.selectButton === '進階護膚') {
          this.currentAllData = items
        } else {
          this.initTab(items, this.num)
        }
      }
    },
    computed: {
      //相依的資料改變時才做計算方法
    },
    methods: {
      toLink(url) {
        window.open(url)
      },
      // 初始
      // 點擊選項
      selectGame(name) {
        this.num = name // 加入active class
        // 清除Tab資料
        this.clearTabData()
        if (name === '全部') this.currentAllData = this.getData
        else this.initTab(this.getData, name)
        // console.log("this.getData", this.getData)
      },
      initTab(obj, name) {
        // 清除Tab資料
        this.clearTabData()
        for (var i in obj) {
          if (obj[i].name === name) {
            this.currentData = obj[i].content
          }
        }
        this.currentTabName = name
      },
      // 清除Tab資料
      clearTabData() {
        this.currentData = []
        this.currentAllData = []
        this.currentTabName = ''
      },
      selectTab(i) {
        // 點擊上方tab i傳入點擊的選項 ex.進階護膚
        this.selectButton = i
        switch (i) {
          case '基礎保養':
            this.getDataItem('maintenance')
            this.getPopularItem('maintenance')
            this.num = '全部'
            break;
          case '進階護膚':
            this.getDataItem('skin')
            this.getPopularItem('skin')
            this.num = '全部'
            break;
          case '防曬':
            this.getDataItem('sunscreen')
            this.num = '臉部防曬'
            break;
          case '底妝':
            this.getDataItem('basicMakeup')
            this.num = '妝前'
            break;
          case '彩妝':
            this.getDataItem('mackeup')
            this.num = '眉彩'
            break;
        }
        // 熱門推薦點擊判斷
        if (i !== '基礎保養' && i !== '進階護膚') {
          $('.popular-wrap').hide()
        } else {
          $('.popular-wrap').show()
        }
      },
      // 取得上方Banner廣告圖
      getBannerItem() {
        let self = this
        this.ajaxSend('images')
          .then(function (res) {
            if (res) {
              // console.log('取得上方Banner廣告圖(success):', res)
              // 中間banner
              self.bannerItem = res.bannerItem
              // 取得banner左方1~6小廣告圖
              self.adItem = res.adItem
              // 取得banner右方1~6小廣告圖
              self.adSubItem = res.adSubItem
            }
          })
          .catch(function (error) {
            console.log('取得上方Banner廣告圖(error):', error)
          })
      },
      // 取得搜尋資料
      getSearchItem(){
        let self = this
        this.ajaxSend('search')
          .then(function (res){
              if (res) {
                if (res.searchItem) self.searchUrl = res.searchItem
                // if (res.searchItem) {
                //   self.search = res.searchItem
                // }
                for(let i in res.searchItem) {
                  self.optionsList.push(res.searchItem[i].text)
                  // 將資料push進去optionsList
                }
              }
          })
          .catch(function (error) {
            console.log('取得search(error):', error)
          })
      },
      // 取得熱門推薦內容
      getPopularItem(type) {
        let self = this
        this.ajaxSend(type)
          .then(function (res) {
            if (res) {
              // console.log('取得熱門推薦內容(success):', res)
              if (res.maintenance_popular) self.getPopular = res.maintenance_popular
              if (res.skin_popular) self.getPopular = res.skin_popular
            }
          })
          .catch(function (error) {
            console.log('取得熱門推薦內容(error):', error)
          })
      },
      // 取得中間分頁內容
      getDataItem(type) {
        let self = this
        this.ajaxSend(type)
          .then(function (res) {
            if (res) {
              // console.log('取得中間分頁內容(success):', res)
              if (res.maintenance_data) self.getData = res.maintenance_data
              if (res.skin_data) self.getData = res.skin_data
              if (res.sunscreen_data) self.getData = res.sunscreen_data
              if (res.basicMakeup_data) self.getData = res.basicMakeup_data
              if (res.mackeup_data) self.getData = res.mackeup_data
            }
          })
          .catch(function (error) {
            console.log('取得中間分頁內容(error):', error)
          })
      },
      // Ajax
      ajaxSend(fileName) {
        return $.ajax({
          url: `/json/${fileName}.json`,
          type: 'GET',
          dataType: 'json' //返回資料格式為json
        });
      }
    },
    //BEGIN--生命週期
    beforeCreate: function () {
      //實體初始化
    },
    created: function () {
      //實體建立完成。資料 $data 已可取得，但 $el 屬性還未被建立。
      this.src = this.$options.__file;
    },
    beforeMount: function () {
      //執行元素掛載之前。
    },
    mounted: function () {
      // 取得上方Banner廣告圖
      this.getBannerItem()
      this.getSearchItem()
      //觸發點擊第一個nav
      $('.nav-item .tab').first().trigger('click');
      $('.nav-item').first().addClass('selected')
      $('.nav-item').click(function () {
        $('.nav-item').removeClass('selected')
        $(this).addClass('selected')
      })

      $('.carousel').carousel({
        interval: 2000
      })

      //gotop
      $(window).scroll(function () {
        if ($(this).scrollTop() > 100) {
          $('.toTop').fadeIn()
        } else {
          $('.toTop').fadeOut()
        }
      })
      $('.toTop').click(function () {
        $('html, body').animate({
          scrollTop: 0
        }, 800)
        return false
      })

      //手機版收合廣告
      $('.pulse-button').click(function () {
        $(".ad").slideToggle("slow");
        if ($(".arrow-2-x").hasClass('x-ready')) {
          $(".collapse-ad").text("展開廣告");
          $(".arrow-2-x").removeClass("x-ready");
          $(".iconarrowdown").removeClass("chevron-on").addClass("chevron-down");
        } else {
          $(".collapse-ad").text("收合廣告");
          $(".arrow-2-x").addClass("x-ready");
          $(".iconarrowdown").removeClass("chevron-down").addClass("chevron-on");
        }
      })
    },
    beforeUpdate: function () {
      //當資料變化時被呼叫，還不會描繪 View。
    },
    updated: function () {
      //當資料變化時被呼叫，還不會描繪 View。
    },
    beforeDestroy: function () {
      //實體還可使用。
    },
    destroyed: function () {
      //實體銷毀。
    }
    //END--生命週期
  }
</script>

<style lang="scss" scoped>

</style>