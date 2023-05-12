<template>
    <ContentBase>
        <div class="row">
            <div class="col-3">
                <UserProfileInfo @follow="follow" @unfollow="unfollow" :user="user" />
                <UserProfileWrite v-if="is_me" @post_a_post="post_a_post" />
            </div>
            <div class="col-9">
                <UserProfilePosts :user="user" :posts="posts" @delete_a_post="delete_a_post" />
            </div>
        </div>
    </ContentBase>
</template>

<script>
import ContentBase from '@/components/ContentBase.vue';
import UserProfileInfo from '@/components/UserProfileInfo.vue';
import UserProfilePosts from '@/components/UserProfilePosts.vue';
import UserProfileWrite from '@/components/UserProfileWrite.vue';
import { reactive } from 'vue';
import { useRoute } from 'vue-router';
import $ from 'jquery';
import { useStore } from 'vuex';
import { computed } from 'vue';

export default {
    name: 'UserProfile',
    components: {
        ContentBase,
        UserProfileInfo,
        UserProfilePosts,
        UserProfileWrite,
    },

    setup() {
        const Route = useRoute();
        const store = useStore();
        const userId = Route.params.userId;

        const user = reactive({
    
        });

        const posts = reactive({
            
        });

        $.ajax({
            url: "https://app165.acapp.acwing.com.cn/myspace/getinfo/",
            type: "GET",
            data:{
                user_id: userId,
            },
            headers: {
                'Authorization': 'Bearer ' + store.state.user.access,
            },
            success(res) {
                user.id = res.id;
                user.username = res.username;
                user.photo = res.photo;
                user.followerCount = res.followerCount;
                user.is_follows = res.is_followed;
            }
        })

        $.ajax({
            url: "https://app165.acapp.acwing.com.cn/myspace/post/",
            type: "GET",
            data: {
                user_id: userId,
            },
            headers: {
                'Authorization': 'Bearer ' + store.state.user.access,
            },
            success(res) {
                console.log(res);
                posts.count = res.length;
                posts.post = res;
            }
        })

        const follow = () => {
            if (user.is_follows) return;
            user.is_follows = true;
            user.followerCount++;
        }

        const unfollow = () => {
            if (!user.is_follows) return;
            user.is_follows = false;
            user.followerCount--;
        }

        const post_a_post = (content) => {
            posts.count++;
            posts.post.unshift({
                id: posts.count,
                user_id: 1,
                content: content,
            })
        }

        const delete_a_post = post_id => {
            posts.post = posts.post.filter(post => post.id !== post_id);
            posts.count = posts.post.length;
        }

        const is_me = computed(() => parseInt(userId) === store.state.user.id);
        
        return {
            user,
            follow,
            unfollow,
            posts,
            post_a_post,
            is_me,
            delete_a_post,
        }
    }
}
</script>

<style scoped>

</style>