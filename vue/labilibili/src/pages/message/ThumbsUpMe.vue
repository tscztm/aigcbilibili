<!--点赞我的-->
<template>
    <div class="wrap">
        <p class="component main-title font-fifth-color">点赞我的</p>
        <el-scrollbar style="height: 90%;">
            <div v-if="replyList && replyList.length > 0" class="component reply-content">
                <!--TODO 修复成有数据的情况-->
                <div v-for="(reply, index) in replyList" :key="index" @click="turnBackDetail(reply)"
                    class="reply-item-wrap" style="position: relative;cursor: pointer;">
                    <!---->
                    <div class="item flex-based-container">
                        <img :src="reply.userCover" class="first-common-avatar" />
                        <div class="flex-column-left-max-container item-content">
                            <div class="reply-item-first-layer flex-based-container">
                                <p class="item-title">{{ reply.nickName }}</p>点赞了你的
                                <p v-if="reply.videoCover">视频</p>
                                <p v-else>评论</p>
                            </div>

                            <div class="flex-based-container">
                                <span class="change-color-btn">{{ reply.createTime }}</span>

                            </div>
                        </div>
                        <div v-if="reply.videoCover"><!--如果是视频，放封面-->
                            <img :src="reply.videoCover" class="video-cover" />
                        </div>
                        <div v-if="reply.videoCover"><!--如果是视频，放封面-->
                            <p class="video-cover">{{ reply.content }}</p>
                        </div>

                    </div>
                    <!-- <div v-if="index !== replyList.length - 1" class="horizontal-divided-line"></div> -->
                </div>
            </div>
            <div v-else>
                <img src="@/assets/img/utils/noData.png" />
            </div>
        </el-scrollbar>
    </div>
</template>

<script setup>
import { onMounted, ref } from "vue"
import { editReplyToRead, fetchReplyNotice, fetchLikeNotice, editLikeToRead } from "@/api/notice"
import { useUserInfo } from "@/store/userInfo"
import { useRouter } from 'vue-router'
const router = useRouter()
const replyList = ref()
const userInfo = useUserInfo() // 使用登录信息
const userId = userInfo.getId()
// 跳转到视频详情页的原数据
const turnBackDetail = (item) => {
    const routeURL = router.resolve({
        name: 'videoDetail',
        params: {
            videoId: item.videoId,
        },
        query: {
            upId: item.userId,
        }
    })
    window.open(routeURL.href, '_blank')
}
// 点赞
const thumbsToReply = (videoId, commentId,) => {

}
// 回复
const replyToReply = () => {

}
// 获取点赞通知的数据
onMounted(async () => {
    await editLikeToRead(userId)

    // 获得数据
    replyList.value = await fetchLikeNotice(userId)
})
</script>

<style lang="scss" scoped>
@import "@/assets/css/messagePage.scss";

.video-cover {
    width: 6rem;
    height: calc(80%);
    position: absolute;
    right: 1rem;
    top: calc(10%);
}

@media screen and (min-width: 1020px) {
    .video-cover {
        width: 8rem;
    }
}
</style>