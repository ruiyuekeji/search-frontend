<template>
  <div>
    <a-input-search
      v-model:value="searchParam.text"
      placeholder="请输入关键字"
      enter-button="检 索"
      size="large"
      @search="onSearch"
    />
    <div class="search-result">
      <a-tabs v-model:activeKey="activeKey" @change="onTabChange">
        <a-tab-pane key="post" tab="文章">
          <PostList />
        </a-tab-pane>
        <a-tab-pane key="img" tab="图片">
          <ImgList />
        </a-tab-pane>
        <a-tab-pane key="user" tab="用户">
          <UserList />
        </a-tab-pane>
      </a-tabs>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watchEffect } from "vue";
import PostList from "@/components/PostList.vue";
import ImgList from "@/components/ImgList.vue";
import UserList from "@/components/UserList.vue";
import { useRoute, useRouter } from "vue-router";

const router = useRouter();
const route = useRoute();
const activeKey = route.params.category
  ? (route.params.category as string)
  : "post";

const initSearchParam = {
  text: "",
  pageSize: 10,
  pageNum: 1,
};

const searchParam = ref(initSearchParam);

watchEffect(() => {
  searchParam.value = {
    ...initSearchParam,
    text: route.query.text as string,
  };
});

const onSearch = (value: string) => {
  router.push({
    query: searchParam.value,
  });
};
const onTabChange = (key: string) => {
  router.push({
    path: `/${key}`,
    query: searchParam.value,
  });
};
</script>

<style scoped>
.search-result {
  margin-top: 30px;
}
</style>
