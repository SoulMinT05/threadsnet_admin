<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar';
import { useToast } from 'vue-toastification';

const statistics = ref({
    currentYear: {
        users: 0,
        posts: 0,
        comments: 0,
        conversations: 0,
    },
    growthRatesYear: {
        users: 0,
        posts: 0,
        comments: 0,
        conversations: 0,
    },
});
const fetchData = async () => {
    try {
        const user = JSON.parse(localStorage.getItem('user') || '{}'); // Xử lý khi không có user
        const userToken = user.accessToken;

        const res = await fetch('http://localhost:3001/api/statistic/year', {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                Authorization: `Bearer ${userToken}`,
            },
        });

        const data = await res.json();

        const toast = useToast();
        if (!data.success) {
            toast.error(data.message);
            return;
        }

        statistics.value = {
            currentYear: {
                users: data.users,
                posts: data.posts,
                comments: data.comments,
                conversations: data.conversations,
            },
            growthRatesYear: {
                users: data.users.growthRate,
                posts: data.posts.growthRate,
                comments: data.comments.growthRate,
                conversations: data.conversations.growthRate,
            },
        };
        console.log('statistics.currentYear: ', statistics.value.currentYear);
    } catch (error) {
        console.error('Error fetching data:', error);
    }
};

const getStatusMessage = (status) => {
    switch (status) {
        case 'pending':
            return 'Đang xử lý';
        case 'accepted':
            return 'Chấp nhận';
        case 'rejected':
            return 'Từ chối';
        default:
            return '';
    }
};

onMounted(() => {
    fetchData(); // Gọi hàm fetchData khi component được mounted
});
</script>

<template>
    <div class="space-y-8">
        <!-- <div v-if="statistics.currentYear.populateComments.length === 0" class="text-center text-gray-500">
            Chưa có đơn hàng
        </div>
        <div v-else> -->
        <div
            v-for="(comment, index) in statistics.currentYear.comments.populateComments"
            :key="index"
            class="flex items-center"
        >
            <Avatar class="h-9 w-9">
                <AvatarImage :src="comment?.userId?.avatar || '/avatars/default.png'" alt="Avatar" />
                <AvatarFallback>
                    {{ comment?.userId?.username || '' }}
                </AvatarFallback>
            </Avatar>
            <div class="ml-4 space-y-1">
                <p class="text-sm font-medium leading-none">
                    {{ comment?.userId?.name || '' }}
                </p>
                <p class="text-sm text-muted-foreground">{{ comment?.userId?.email || '' }}</p>
            </div>
            <div class="ml-auto">
                <span class="px-3 py-1 rounded-full text-sm font-medium">
                    <!-- {{ getStatusMessage(order.status) }} -->
                    {{ comment.textComment }}
                </span>
            </div>
        </div>
        <!-- </div> -->
    </div>
</template>
