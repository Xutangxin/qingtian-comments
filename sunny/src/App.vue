<template>
  <div class="main">
    <!-- 标题 -->
    <div id="title">
      <h1 class="white-words">-晴天の网易云评论-</h1>
    </div>

    <!-- 视频 -->
    <div id="video">
      <iframe
        src="http://player.bilibili.com/player.html?aid=328746951&bvid=BV1VA411e7PM&cid=208118542&page=1"
        scrolling="no"
        border="0"
        frameborder="no"
        framespacing="0"
        allowfullscreen="true"
      ></iframe>
    </div>

    <!-- 评论部分 -->
    <div id="comments">
      <!-- 热评 -->
      <div id="hot-comments">
        <h2 class="white-words">|热评 TOP15</h2>

        <div id="comments-detail">
          <div id="comments-item" v-for="(item, index) in hotComments" :key="index">
            <div id="img">
              <img :src="item.user.avatarUrl" alt />
            </div>
            <div id="comment-info">
              <div id="words">{{item.content}}</div>
              <span id="user">
                {{item.user.nickname}}
                <br />
                {{timestampToTime(item.time)}}
              </span>
            </div>
          </div>
        </div>
      </div>
      <!-- 非热评 -->
      <div id="other-comments">
        <h2 class="white-words">|其他评论</h2>
        <div id="comments-detail">
          <div id="comments-item" v-for="(item, index) in otherComments" :key="index">
            <div id="img">
              <img :src="item.user.avatarUrl" alt />
            </div>
            <div id="comment-info">
              <div id="words">{{item.content}}</div>
              <span id="user">
                {{item.user.nickname}}
                <br />
                {{timestampToTime(item.time)}}
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- 显示结束 -->
    <div id="end">
      <h3 class="white-words">再也没有了哦</h3>
    </div>
  </div>
</template>

<script>
// import axios from "axios";
import comments from "./comments.json";
export default {
  name: "App",

  data() {
    return {
      // offset: 0,
      hotComments: comments.hotComments,
      otherComments: comments.comments,
      datas: null,
    };
  },

  methods: {
    // 格式化时间
    //将时间戳转换为 年-月-日 的格式
    timestampToTime(timestamp) {
      let date = new Date(timestamp); //时间戳为10位需*1000，时间戳为13位的话不需乘1000
      let Y = date.getFullYear() + "-";
      let M =
        (date.getMonth() + 1 < 10
          ? "0" + (date.getMonth() + 1)
          : date.getMonth() + 1) + "-";
      let D = date.getDate() + " ";
      return Y + M + D;
    },
  },

  created() {},

  mounted() {
    console.log(this.hotComments);
    console.log(this.otherComments);
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #c82e35;
  font-family: "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell",
    "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
}

.white-words {
  color: #fffefe;
}

.main {
  width: 100%;
  min-width: 1000px;
  padding: 20px;
  background-color: #c82e35;
}

#title {
  width: 30%;
  margin: 10px auto 20px;
  text-align: center;
}

/* 视频部分 */
#video {
  width: 45%;
  height: 430px;
  margin: 0 auto 10px;
  padding: 0;
}
#video iframe {
  width: 100%;
  height: 100%;
}

/* 评论部分 */
#hot-comments {
  width: 100%;
  /* border: 1px solid pink; */
}

#hot-comments h2,
#other-comments h2 {
  margin-bottom: 13px;
}

#comments-detail {
  display: flex;
  /* flex-direction: column; */
  justify-content: space-around;
  flex-wrap: wrap;
  width: 100%;
}

#other-comments {
  width: 100%;
  /* border: 1px solid gray; */
}

/* 每条评论 */
#comments-item {
  display: flex;
  width: 19%;
  height: fit-content;
  margin-bottom: 5px;
  padding: 5px;
  background-color: #fffdff;
  border-radius: 10px;
  overflow: hidden;
}

#comments-item #img {
  flex: 1;
  text-align: center;
}
#img img {
  width: 60%;
  margin: 5px auto 0;
  border-radius: 50%;
}

#comments-item #comment-info {
  flex: 3;
  padding: 5px;
}

#comment-info #words {
  font-size: 14px;
}

#comments-item #comment-info #user {
  color: #b2b1b2;
  font-size: 12px;
  text-align: right;
}

#end {
  width: 20%;
  height: 30px;
  margin: 20px auto 0;
  text-align: center;
}
</style>