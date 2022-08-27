<template>
  <div class="comments-item">
    <div class="img">
      <img :src="data.user.avatarUrl" alt />
    </div>
    <div class="comment-info">
      <div class="text">{{ data.content }}</div>
      <div class="user">
        <p>
          {{ data.user.nickname }}
        </p>
        <p>
          {{ timestampToTime(data.time) }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CommentCard',
  props: {
    data: {
      type: Object,
      default: () => ({})
    }
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
    }
  }
};
</script>

<style scoped>
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
</style>
