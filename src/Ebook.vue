<template>
  <div class="ebook">
    <title-bar
      :showTitleAndMenu="showTitleAndMenu">
    </title-bar>
    <div class="read-wrapper">
      <div id="read"></div>
      <div class="mask">
        <div class="left" @click="prevPage"></div>
        <div class="center" @click="toggleTitleAndMenuShow"></div>
        <div class="right" @click="nextPage"></div>
      </div>
    </div>
    <menu-bar
      :showTitleAndMenu="showTitleAndMenu"
      :fontSizeList="fontSizeList"
      :defaultFontSize="defaultFontSize"
      @setFontSize="setFontSize"
      ref="menuBar"
    ></menu-bar>
  </div>
</template>

<script>
import TitleBar from '@/components/TitleBar'
import MenuBar from '@/components/MenuBar'
import Epub from 'epubjs'
const DOWNLOAD_URL = '/static/2018_Book_AgileProcessesInSoftwareEngine.epub'
global.ePub = Epub
export default {
  components: {
    TitleBar,
    MenuBar
  },
  data() {
    return {
      showTitleAndMenu: false,
      fontSizeList: [
        { fontSize: 12 },
        { fontSize: 14 },
        { fontSize: 16 },
        { fontSize: 18 },
        { fontSize: 20 },
        { fontSize: 22 },
        { fontSize: 24 }
      ],
      defaultFontSize: 16
    }
  },
  methods: {
    toggleTitleAndMenuShow() {
      this.showTitleAndMenu = !this.showTitleAndMenu
      if (!this.showTitleAndMenu) {
        this.$refs.menuBar.hideSetting()
      }
    },
    // 电子书的解析和渲染
    showEpub() {
      // 生成ebook
      this.book = new Epub(DOWNLOAD_URL)
      // 生成Rendition
      this.rendition = this.book.renderTo('read', {
        width: window.innerWidth,
        height: window.innerHeight
      })
      // 生成Rendition.display渲染电子书
      this.rendition.display()
      // 获取theme对象
      this.themes = this.rendition.themes
      // 设置默认字体大小
      this.setFontSize(this.defaultFontSize)
    },
    prevPage() {
      // Rendtion.prev
      if (this.rendition) {
        this.rendition.prev()
      }
    },
    nextPage() {
      // Rendtion.next
      if (this.rendition) {
        this.rendition.next()
      }
    },
    setFontSize(fontSize) {
      this.defaultFontSize = fontSize
      if (this.themes) {
        this.themes.fontSize(fontSize + 'px')
      }
    }
  },
  mounted() {
    this.showEpub()
  }
}
</script>

<style lang="scss" scoped>
  @import 'assets/styles/global';
  .ebook {
    position: relative;
    .read-wrapper {
      .mask {
        position: absolute;
        top: 0;
        left: 0;
        display: flex;
        width: 100%;
        height: 100%;
        z-index: 100;
        .left {
          flex: 0 0 px2rem(100);

        }
        .center {
          flex: 1;

        }
        .right {
          flex: 0 0 px2rem(100);

        }
      }
    }
  }
</style>
