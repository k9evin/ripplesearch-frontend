<template>
  <div class="index-page">
    <a-input-search
      v-model:value="searchParams.text"
      placeholder="input search text"
      enter-button="Search"
      size="large"
      @search="onSearch"
    />
    <MyDivider />
    <a-tabs v-model:activeKey="activeKey" @change="onTabChange">
      <a-tab-pane key="post" tab="帖子">
        <PostList :post-list="postList" />
      </a-tab-pane>
      <a-tab-pane key="picture" tab="图片">
        <PictureList />
      </a-tab-pane>
      <a-tab-pane key="user" tab="用户">
        <UserList :user-list="userList" />
      </a-tab-pane>
    </a-tabs>
  </div>
</template>

<script setup lang="ts">
import { ref, watchEffect } from "vue";
import PostList from "@/components/PostList.vue";
import PictureList from "@/components/PictureList.vue";
import UserList from "@/components/UserList.vue";
import MyDivider from "@/components/MyDivider.vue";
import { useRouter, useRoute } from "vue-router";
import myAxios from "@/plugins/myAxios";
import { JSONSchema } from "@typescript-eslint/utils";

const postList = ref([]);

myAxios.post("post/list/page/vo", {}).then((res) => {
  postList.value = res.records;
});

const userList = ref([]);

myAxios.post("/user/list/page/vo", {}).then((res) => {
  userList.value = res.records;
});

const router = useRouter();
const route = useRoute();

let activeKey = route.params.category;
const initSearchParams = {
  text: "",
  pageSize: 10,
  pageNum: 1,
};
const searchParams = ref({ text: "", pageSize: 10, pageNum: 1 });

watchEffect(() => {
  searchParams.value = {
    ...initSearchParams,
    text: route.query.text,
  } as any;
  activeKey = route.params.category;
});

const onSearch = (value: string) => {
  router.push({
    query: searchParams.value,
  });
};

const onTabChange = (key: string) => {
  console.log(key);
  router.push({
    path: `/${key}`,
    query: searchParams.value,
  });
};
</script>
