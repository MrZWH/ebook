<template>
	<div class="ebook">
		<title-bar :ifTitleAndMenuShow="ifTitleAndMenuShow"></title-bar>
		<div class="read-wrapper">
			<div id="read"></div>
			<div class="mask">
				<div class="left" @click="prevPage"></div>
				<div class="center" @click="toggleTitleAndMenu"></div>
				<div class="right" @click="nextPage"></div>
			</div>
		</div>
		<menu-bar
      @setFontSize="setFontSize"
      :defaultFontSize="defaultFontSize"
      :fontSizeList="fontSizeList"
      :ifTitleAndMenuShow="ifTitleAndMenuShow"
      ref="menuBar"
      :themeList="themeList"
      :defaultTheme="defaultTheme"
      @setTheme="setTheme"
    ></menu-bar>
	</div>
</template>

<script>
import TitleBar from '@/components/TitleBar'
import MenuBar from "@/components/MenuBar";
import Epub from "epubjs";
const DOWNLOAD_URL = "/static/2018_Book_AgileProcessesInSoftwareEngine.epub";
global.ePub = Epub;

export default {
	components: {
		TitleBar,
		MenuBar
	},
  data() {
    return {
      ifTitleAndMenuShow: false,
      fontSizeList: [
        {fonstSize: 12},
        {fonstSize: 14},
        {fonstSize: 16},
        {fonstSize: 18},
        {fonstSize: 20},
        {fonstSize: 22},
        {fonstSize: 24},
      ],
      defaultFontSize: 16,
      themeList: [
        {
          name: 'default',
          style: {
            body: {
              'color': '#000', 'background': '#fff'
            }
          }
        },
        {
          name: 'eye',
          style: {
            body: {
              'color': '#000', 'background': '#ceeaba'
            }
          }
        },
        {
          name: 'night',
          style: {
            body: {
              'color': '#fff', 'background': '#000'
            }
          }
        },
        {
          name: 'gold',
          style: {
            body: {
              'color': '#000', 'background': 'rgb(241, 236, 226)'
            }
          }
        }
      ],
      defaultTheme: 0
    };
  },
  methods: {
    setTheme(index) {
      this.themes.select(this.themeList[index].name)
      this.defaultTheme = index
    },
    registerTheme() {
      this.themeList.forEach(theme => {
        this.themes.register(theme.name, theme.style)
      });
    },
    setFontSize(fontSize) {
      this.defaultFontSize = fontSize;
      if (this.themes) {
        this.themes.fontSize(fontSize + 'px')
      }
    },
    toggleTitleAndMenu() {
			this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow;
			if(!this.ifTitleAndMenuShow) {
				this.$refs.menuBar.hideSetting()
			}
    },
    prevPage() {
      // Rendition.prev
      if (this.rendition) {
        this.rendition.prev();
      }
    },
    nextPage() {
      if (this.rendition) {
        this.rendition.next();
      }
    },
    // 电子书的解析和渲染
    showEpub() {
      // 生成 Book 对象
      this.book = new Epub(DOWNLOAD_URL);
      // 生成 Rendition 通过 Book.renderTo
      this.rendition = this.book.renderTo("read", {
        width: window.innerWidth,
        height: window.innerHeight
      });
      // 通过 Rendition.display 渲染电子书
      this.rendition.display();

      // 获取 Theme 对象
      this.themes = this.rendition.themes
      // 设置默认字体
      this.setFontSize(this.defaultFontSize)
      // this;themes.register(name, styles)
      // this.themes.select(name)
      this.registerTheme()
      this.setTheme(this.defaultTheme)
    }
  },
  mounted() {
    this.showEpub();
  }
};
</script>

<style lang='scss' scoped>
@import "assets/styles/global";

.ebook {
  position: relative;
  .read-wrapper {
    .mask {
      position: absolute;
      top: 0;
      left: 0;
      z-index: 100;
      width: 100%;
      height: 100%;
      display: flex;
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