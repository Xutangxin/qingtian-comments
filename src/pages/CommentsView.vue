<template>
  <div class="container">
    <div class="title">
      <h1 class="white-text">-晴天の网易云评论-</h1>
    </div>

    <div class="video">
      <iframe
        src="https://www.bilibili.com/video/BV1Yv4y1Z7km/?spm_id_from=333.337.search-card.all.click&vd_source=f597b496d3bd92bba0b88a4aab875720"
        scrolling="no"
        border="0"
        frameborder="no"
        framespacing="0"
        allowfullscreen="true"
      ></iframe>
    </div>

    <div class="comments">
      <div class="hot-comments">
        <h2 class="white-text">|热评 TOP15</h2>
        <div class="comments-detail">
          <comment-card
            v-for="(item, index) in hotComments"
            :key="index"
            :data="item"
          ></comment-card>
        </div>
      </div>

      <div class="other-comments">
        <h2 class="white-text">|评论回忆</h2>
        <div class="comments-detail">
          <comment-card
            v-for="(item, index) in otherComments"
            :key="index"
            :data="item"
          ></comment-card>
        </div>
      </div>
    </div>

    <div class="load-more" @click="getData">
      <div :class="{ 'no-more': noMore }">
        {{ noMore ? '没有更多了哦' : '加载更多' }}
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import CommentCard from '../components/CommentCard.vue';
export default {
  components: { CommentCard },
  name: 'CommentsView',
  comments: {
    CommentCard
  },
  data() {
    return {
      hotComments: [],
      otherComments: [],
      limit: 80,
      offset: 0,
      noMore: false
    };
  },

  created() {
    this.getData();
  },

  methods: {
    async getData() {
      if (this.noMore) return;
      const res = await axios({
        url: '/api',
        method: 'get',
        params: {
          limit: this.limit,
          offset: this.offset
        }
      });
      const {
        data: { code, comments, hotComments }
      } = res;

      if (code && code === 200) {
        if (!comments.length) {
          this.noMore = true;
        }
        hotComments && (this.hotComments = hotComments);
        this.otherComments.push(...comments);
        this.offset += this.limit;
      }
    }
  }
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', 'Roboto', 'Oxygen', 'Ubuntu', 'Cantarell',
    'Fira Sans', 'Droid Sans', 'Helvetica Neue', sans-serif;
}

.white-text {
  color: #fff;
}

.container {
  min-width: 1000px;
  padding: 20px;
  background-image: linear-gradient(
    to bottom right,
    #ff3333c2,
    rgb(236, 236, 144)
  );
}

.title {
  width: 30%;
  margin: 10px auto 20px;
  text-align: center;
}

.video {
  width: 50%;
  height: 430px;
  margin: 0 auto 10px;
  padding: 0;
}
.video iframe {
  width: 100%;
  height: 100%;
  border-radius: 5px;
}

.hot-comments h2,
.other-comments h2 {
  margin-bottom: 13px;
}

.comments-detail {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
  width: 100%;
}

.load-more {
  margin: 20px auto 0;
  text-align: center;
}
.load-more div {
  width: fit-content;
  margin: 0 auto;
  cursor: pointer;
  border-radius: 5px;
  color: #fff;
  padding: 8px 14px;
  transition: 0.3s;
  font-weight: 600;
}
.load-more div.no-more {
  cursor: default;
}
.load-more div:not(.no-more):hover {
  background: #fff;
  color: #ff3333c2;
}
</style>
