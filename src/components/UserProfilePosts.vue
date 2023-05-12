<template>
<div class="card">
    <div class="car-body">
        <div v-for="post in posts.post" :key="post.id">
            <div class="card single-post">
                <div class="card-body">
                    {{ post.content }}
                    <button v-if="is_me" @click="delete_a_post(post.id)" type="button" class="btn btn-danger">删除</button>
                </div>
            </div>
        </div>
    </div>
</div>

</template>

<script>
import $ from 'jquery';
import { useStore } from 'vuex';
import { computed } from 'vue';

export default {
    name: 'UserProfilePosts',

    props: {
        posts: {
            type: Object,
            required:true,
        },
        user: {
            type: Object,
            required:true,
        }
    },

    setup(props, context) {
        const store = useStore();
        const is_me = computed(() => store.state.user.id === props.user.id);
        const delete_a_post = (post_id) => {
            $.ajax({
            url: "https://app165.acapp.acwing.com.cn/myspace/post/",
            type: "DELETE",
            data: {
                post_id:post_id
            },
            headers: {
                'Authorization': 'Bearer ' + store.state.user.access,
            },
            success(res) {
                if (res.result === 'success')
                    context.emit('delete_a_post', post_id);
            }
        })
        }

        return {
            is_me,
            delete_a_post,
        }
    }
}

</script>

<style scoped>
.single-post {
    margin: 10px;
}

.btn-danger{
    float: right;
}
</style>