<template>
    <ContentBase>
        <div class="card" v-for="user in users" :key="user.id" @click="open_a_userprofile(user.id)">
            <div class="card-body">
                <div class="row">
                    <div class="col-1">
                        <img class="img-fluid" :src="user.photo"  />
                    </div>
                    <div class="col-11">
                        <div class="username">{{ user.username }}</div>
                        <div class="followercount">粉丝：{{ user.followerCount }}</div>
                    </div>
                </div>
            </div>
        </div>
    </ContentBase>
</template>

<script>
import ContentBase from '@/components/ContentBase.vue';
import $ from 'jquery';
import { ref } from 'vue';
import router from '@/router';
import { useStore } from 'vuex';

export default {
    name: 'UserList',
    components: {
        ContentBase,
    },

    setup() {
        let users = ref([]);
        const store = useStore();

        $.ajax({
            url: 'https://app165.acapp.acwing.com.cn/myspace/userlist/',
            type: "get",
            success(res) {
                users.value = res;
            }
        });

        const open_a_userprofile = userId => {
            if (store.state.user.is_login) {
                router.push({ name: 'userprofile', params: { userId } });
            } else {
                router.push({ name: 'login' });
            }
        };

        return {
            users,
            open_a_userprofile,
        }
    }
}
</script>

<style scoped>
img {
    border-radius: 50%;
}

.username {
    font-weight: bold;
    height: 50%;
}

.followercount{
    font-size: 12px;
    color: grey;
    height: 50%;
}

.card{
    margin: 10px;
    cursor: pointer;
}

.card:hover {
    box-shadow: 2px 2px 10px lightgray;
    transition: 500ms;
}
</style>