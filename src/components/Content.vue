<template>
  <transition name="slide-right">
    <div class="content">
      <div class="content-bookmark">
        <div
          class="changeTag"
          :class="{'selected': chooseContentOrBookmark === true}"
          @click="chooseContent"
        >
          {{ $t('menu.content') }}
        </div>
        <div
          class="changeTag"
          :class="{'selected': chooseContentOrBookmark === false}"
          @click="chooseBookmark"
        >
          {{ $t('menu.bookmark') }}
        </div>
      </div>
      <div
        class="content-wrapper"
        v-if="bookAvailable"
        v-show="chooseContentOrBookmark"
      >
        <vue-scroll>
          <div
            class="content-item"
            v-for="(item, index) in navigation.toc"
            :key="index"
            @click="jumpTo(item.href)"
          >
            <span class="text">
              {{ item.label }}
            </span>
          </div>
        </vue-scroll>
      </div>
      <div
        class="empty"
        v-else
        v-show="chooseContentOrBookmark"
      >
        {{ $t('default.loading') }}
      </div>
      <div
        class="content-wrapper"
        v-if="bookmarks"
        v-show="!chooseContentOrBookmark"
      >
        <vue-scroll>
          <div
            class="content-item"
            v-for="(item, index) in bookmarks"
            :key="index"
            @click="turnMarkPage(index)"
          >
            <span class="text">
              {{ item.title }}
            </span>
          </div>
        </vue-scroll>
      </div>
      <div
        class="empty"
        v-else
        v-show="!chooseContentOrBookmark"
      >
        {{ $t('default.wait4mark') }}
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'ContentView',
  data () {
    return {
      chooseContentOrBookmark: true
    }
  },
  props: {
    ifShowContent: Boolean
  },
  methods: {
    jumpTo (target) {
      this.$emit('jumpTo', target)
    },
    turnMarkPage (index) {
      this.$emit('turnMarkPage', index)
    },
    chooseContent () {
      this.chooseContentOrBookmark = true
    },
    chooseBookmark () {
      this.chooseContentOrBookmark = false
    }
  },
  computed: {
    navigation () {
      return this.$store.state.navigation
    },
    bookmarks () {
      return this.$store.state.bookmarks
    },
    bookAvailable () {
      return this.$store.state.bookAvailable
    }
  }
}
</script>

<style lang="scss" scoped>
  @import "../styles/global";

  .content {
    position: absolute;
    display: flex;
    flex-direction: column;
    top: 0;
    left: 0;
    width: 80%;
    height: 100%;
    z-index: 102;
    background: #fff;
    box-shadow: px2rem(8) 0 px2rem(8) rgba(0, 0, 0, .15);
    .content-bookmark {
      display: flex;
      width: 100%;
      height: px2rem(48);
      box-shadow: px2rem(8) 0 px2rem(8) rgba(0, 0, 0, .15);
      .changeTag {
        cursor: pointer;
        flex: 1;
        @include center;
        font-size: px2rem(20);
        &.selected {
          background: #e8eaec;
        }
      }
    }
    .content-wrapper {
      width: 100%;
      height: 100%;
      overflow: auto;
      .content-item {
        cursor: pointer;
        padding: px2rem(15) px2rem(15);
        border-bottom: px2rem(1) solid #f4f4f4;
        display: flex;
        align-items: center;
        .text {
          font-size: px2rem(16);
          color: #272822;
        }
      }
    }
    .empty {
      width: 100%;
      height: 100%;
      @include center;
      font-size: px2rem(16);
      color: #272822;
    }
    &.slide-right-enter, &.slide-right-leave-to {
      transform: translate3d(-100%, 0, 0);
    }
    &.slide-right-enter-to, &.slide-right-leave {
      transform: translate3d(0, 0, 0);
    }
    &.slide-right-enter-active, &.slide-right-leave-active {
      transition: all .3s linear;
    }
  }
</style>
