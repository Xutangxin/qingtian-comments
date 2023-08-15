<template>
  <div>
    <comments-view></comments-view>
  </div>
</template>

<script>
import CommentsView from './pages/CommentsView.vue';
export default {
  name: 'App',
  components: {
    CommentsView
  }
};
</script>

<style lang="scss" scoped></style>

<style>
body {
  margin: 0;
}
</style>
