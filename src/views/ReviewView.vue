<template>
  <v-app>
    <!-- 헤더 -->
    <!-- <Header /> -->

    <!-- 리뷰 카드 영역 -->
    <v-main>
      <v-container
        class="fill-height d-flex justify-center align-center pt-0"
        fluid
      >
        <!-- ReviewCard 자체가 카드 스타일 담당 -->
        <ReviewCard 
        :menuIndex="menuStore.selectedMenuIndex" 
        />
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup>
import { onMounted, watch } from 'vue'
import { useRoute } from 'vue-router'
import { useLogStore } from '../store/logStore'
import { useMenuStore } from '../store/menuStore'
import { toKSTDateTime } from '../utils/timeUtil'
import { getOrCreateUUID } from '../utils/uuidUtil'
import Header from '../components/Header.vue'
import ReviewCard from '../components/ReviewCard.vue'

const route = useRoute()

const logStore = useLogStore()
const menuStore = useMenuStore()

const logReviewView = () => {
  const uuid = getOrCreateUUID()

  logStore.addLog({
    user_id: uuid,
    event_name: 'view_review_screen',
    event_value: {},
    page_name: 'review_view',
    event_time: toKSTDateTime(new Date()),
  })
}

onMounted(() => {
  logReviewView()
})

watch(
  () => route.fullPath,
  (newPath, oldPath) => {
    if (newPath.startsWith('/review') && oldPath !== newPath) {
      logReviewView()
    }
  }
)
</script>
