<template>
  <div class="mu-infinite-scroll">
    <circular v-show="loading" :size="24"></circular>
    <span v-show="loading" class="mu-infinite-scroll-text">{{loadingText}}</span>
  </div>
</template>

<script>
import circular from '../internal/circular'
import Scroll from '../internal/scroll'
export default {
  name: 'mu-infinite-scroll',
  mixins: [Scroll],
  props: {
    loading: {
      type: Boolean,
      default: false
    },
    loadingText: {
      type: String,
      default: '正在加载....'
    },
    isLoaded: {
      type: Boolean,
      default: false
    },
    type: {
      type: String,
    },
  },
  data() {
    return {
      last: 0,      //  上一次scrollTop
      present: 0,   //  这一次scrollTop
      lock: false,
    }
  },
  methods: {
    onScroll(e) {
      if (this.loading || this.isLoaded) return
      const scroller = this.scroller
      const isWindow = scroller === window
      const scrollTop = isWindow ? scroller.scrollY : scroller.scrollTop
      const scrollHeight = isWindow ? document.documentElement.scrollHeight || document.body.scrollHeight : scroller.scrollHeight
      let h = scrollHeight - scrollTop - 5
      let sh = isWindow ? window.innerHeight : scroller.offsetHeight
      if (h <= sh) {
        this.$emit('load')
      }

      if (!this.lock) {
        this.lock = true;
        this.present = e.target.scrollTop;
        let distance = this.present - this.last;
        if (distance > 0) {
          this.$emit('scrolldown', this.present, this.type);
        } else if (distance < 0) {
          this.$emit('scrollup', this.present, this.type);
        }
        setTimeout(function () {
          this.last = this.present;
          this.lock = false;
        }.bind(this), 50);
      }
    }
  },
  components: {
    circular
  }
}
</script>

<style lang="css">
.mu-infinite-scroll {
  display: flex;
  justify-content: center;
  align-items: center;
  padding-bottom: 8px;
  line-height: 36px;
  width: 100%;
}

.mu-infinite-scroll-text {
  margin-left: 16px;
  font-size: 13px;
  color: #fff;
}
</style>
