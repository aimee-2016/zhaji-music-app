<template>
  <div class="singer" ref="singer">
    <!-- <list-view @select="selectSinger" :data="singers" ref="list"></list-view> -->
    <router-view></router-view>
  </div>
</template>

<script>
// import ListView from 'base/listview/listview'
import { getSingerList } from 'api/singer'
import { ERR_OK } from 'api/config'
import Singer from 'common/js/singer'
// import {mapMutations} from 'vuex'
// import { playlistMixin } from 'common/js/mixin'

// 常量全部用大写命名
const HOT_SINGER_LEN = 10
const HOT_NAME = '热门'

export default {
  // mixins: [playlistMixin],
  data () {
    return {
      singers: []
    }
  },
  created () {
    this._getSingerList()
  },
  methods: {
    handlePlaylist (playlist) {
      const bottom = playlist.length > 0 ? '60px' : ''
      this.$refs.singer.style.bottom = bottom
      this.$refs.list.refresh()
    },
    selectSinger (singer) {
      this.$router.push({
        path: `/singer/${singer.id}`
      })
      this.setSinger(singer)
    },
    _getSingerList () {
      getSingerList().then((res) => {
        if (res.code === ERR_OK) {
          this.singers = this._normalizeSinger(res.data.list)
          console.log(this.singers)
        }
      })
    },
    _normalizeSinger (list) {
      let map = {
        hot: {
          title: HOT_NAME,
          item: []
        }
      }
      list.forEach((element, index) => {
        if (index < HOT_SINGER_LEN) {
          map.hot.item.push(new Singer({name: element.Fsinger_name, id: element.Fsinger_id}))
        }
        let key = element.Findex
        if (!map[key]) {
          map[key] = {
            title: key,
            item: []
          }
        }
        map[key].item.push(new Singer({name: element.Fsinger_name, id: element.Fsinger_id}))
      })
      let ret = []
      let hot = []
      for (let i in map) {
        let val = map[i]
        if (val.title === HOT_NAME) {
          hot.push(val)
        }
        if (val.title.match(/[a-zA-Z]/)) {
          ret.push(val)
        }
      }
      ret.sort((a, b) => {
        return a.title.charCodeAt(0) - b.title.charCodeAt(0)
      })
      return hot.concat(ret)
    }
    // ...mapMutations({
    //   setSinger: 'SET_SINGER'
    // })
  }
  // components: {
  //   ListView
  // }
}

</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
.singer {
  position: fixed;
  top: 88px;
  bottom: 0;
  width: 100%;
}
</style>
