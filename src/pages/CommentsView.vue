<template>
  <div class="main">
    <div class="title">
      <h1 class="white-text">-晴天の网易云评论-</h1>
    </div>

    <div class="video">
      <iframe
        src="http://player.bilibili.com/player.html?aclass=328746951&bvclass=BV1VA411e7PM&cclass=208118542&page=1"
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
          <div
            class="comments-item"
            v-for="item in hotComments"
            :key="item.commentId"
          >
            <div class="img">
              <img :src="item.user.avatarUrl" alt />
            </div>
            <div class="comment-info">
              <div class="text">{{ item.content }}</div>
              <div class="user">
                <p>
                  {{ item.user.nickname }}
                </p>
                <p>
                  {{ timestampToTime(item.time) }}
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="other-comments">
        <h2 class="white-text">|评论回忆</h2>
        <div class="comments-detail">
          <div
            class="comments-item"
            v-for="item in otherComments"
            :key="item.commentId"
          >
            <div class="img">
              <img :src="item.user.avatarUrl" alt />
            </div>
            <div class="comment-info">
              <div class="text">{{ item.content }}</div>
              <div class="user">
                <p>{{ item.user.nickname }}</p>
                <p>{{ timestampToTime(item.time) }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="load-more" @click="getData">
      <span :class="{ 'no-more': noMore }">{{
        noMore ? '没有更多了哦' : '加载更多'
      }}</span>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'CommentsView',

  data() {
    return {
      hotComments: [],
      otherComments: [],
      limit: 80,
      offset: 0,
      noMore: false
    };
  },

  methods: {
    //将时间戳转换为 年-月-日 的格式
    timestampToTime(timestamp) {
      const date = new Date(timestamp);
      const year = date.getFullYear();
      const m = date.getMonth() + 1;
      const month = m < 10 ? '0' + m : m;
      const day = date.getDate() < 10 ? '0' + date.getDate() : date.getDate();
      return `${year}-${month}-${day}`;
    },

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
  },

  created() {
    this.getData();
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
  background: #c82e35;
  font-family: 'Segoe UI', 'Roboto', 'Oxygen', 'Ubuntu', 'Cantarell',
    'Fira Sans', 'Droid Sans', 'Helvetica Neue', sans-serif;
}

.white-text {
  color: #fff;
}

.main {
  min-width: 1000px;
  padding: 20px;
  background: #c82e35;
  border-radius: 10px;
}

.title {
  width: 30%;
  margin: 10px auto 20px;
  text-align: center;
}

.video {
  width: 45%;
  height: 430px;
  margin: 0 auto 10px;
  padding: 0;
}
.video iframe {
  width: 100%;
  height: 100%;
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

.comments-item {
  display: flex;
  width: 19%;
  height: fit-content;
  margin-bottom: 8px;
  padding: 5px;
  background: #ffff;
  border-radius: 10px;
  overflow: hidden;
  cursor: default;
  transition: 0.2s;
}
.comments-item:hover {
  box-shadow: #fff 0 0 18px;
}

.comments-item .img {
  flex: 1;
  text-align: center;
}
.img img {
  width: 60%;
  margin: 5px auto 0;
  border-radius: 50%;
}

.comments-item .comment-info {
  flex: 3;
  padding: 5px;
}

.comment-info .text {
  font-size: 14px;
}

.comments-item .comment-info .user {
  margin-top: 3px;
  color: #b2b1b2;
  font-size: 12px;
  text-align: right;
}
.comments-item .comment-info .user p {
  text-align: left;
}
.end {
  width: 20%;
  height: 30px;
  margin: 20px auto 0;
  text-align: center;
}

.load-more {
  margin: 20px auto 0;
  text-align: center;
}
.load-more span {
  cursor: pointer;
  border: 1px solid lightgray;
  border-radius: 5px;
  color: #fff;
  padding: 8px 14px;
  transition: 0.3s;
}
.load-more span.no-more {
  cursor: default;
}
.load-more span:not(.no-more):hover {
  font-weight: 600;
  background: #fff;
  color: #c82e35;
}
</style>
