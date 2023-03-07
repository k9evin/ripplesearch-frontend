<template>
  <a-list item-layout="horizontal" :data-source="userList">
    <template #renderItem="{ item }">
      <a-card hoverable style="width: 240px">
        <template #cover>
          <img :src="cxk" alt="gege" />
        </template>
        <a-card-meta :title="item.userName">
          <template #description>{{ item.userProfile }}</template>
        </a-card-meta>
      </a-card>
    </template>
  </a-list>
</template>

<script setup lang="ts">
import { defineProps, withDefaults } from "vue";
import cxk from "@/assets/cxk.jpeg";

interface Props {
  userList: any[];
}

const props = withDefaults(defineProps<Props>(), {
  userList: () => [],
});
</script>

<style scoped></style>
