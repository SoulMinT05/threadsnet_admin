<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar';
import { useToast } from 'vue-toastification';

const statistics = ref({
    currentWeek: {
        users: 0,
        products: 0,
        orders: 0,
        publishers: 0,
        populateOrders: [], // Thêm trường này để lưu đơn hàng
    },
    growthRates: {
        users: 0,
        products: 0,
        orders: 0,
        publishers: 0,
    },
});

const fetchData = async () => {
    try {
        const user = JSON.parse(localStorage.getItem('user') || '{}'); // Xử lý khi không có user
        const userToken = user.accessToken;

        const res = await fetch('http://localhost:3001/api/statistic/week', {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                Authorization: `Bearer ${userToken}`,
            },
        });

        const data = await res.json();
        console.log('dataRecentWeek: ', data);

        const toast = useToast();
        if (!data.success) {
            toast.error(data.message);
            return;
        }

        // Lấy số liệu từ tuần đầu tiên
        const firstWeekData = data.statisticsWeek[0];

        statistics.value = {
            currentWeek: {
                users: firstWeekData.users.count,
                products: firstWeekData.products.count,
                orders: firstWeekData.orders.count,
                publishers: firstWeekData.publishers.count,
                populateOrders: firstWeekData.orders.populateOrders || [], // Giả định rằng `populateOrders` nằm trong `orders`
            },
            growthRates: {
                users: firstWeekData.users.growthRate,
                products: firstWeekData.products.growthRate,
                orders: firstWeekData.orders.growthRate,
                publishers: firstWeekData.publishers.growthRate,
            },
        };
        console.log(' statistics.valueWeeeeeek: ', statistics.value);
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
        <div v-if="statistics.currentWeek.populateOrders.length === 0" class="text-center text-gray-500">
            Chưa có đơn hàng
        </div>
        <div v-else>
            <div
                v-for="(order, index) in statistics.currentWeek.populateOrders"
                :key="index"
                class="flex items-center mt-4"
            >
                <Avatar class="h-9 w-9">
                    <AvatarImage :src="order.orderBy.avatarUrl || '/avatars/default.png'" alt="Avatar" />
                    <AvatarFallback>
                        {{ order.orderBy.firstName.charAt(0) }}{{ order.orderBy.lastName.charAt(0) }}
                    </AvatarFallback>
                </Avatar>
                <div class="ml-4 space-y-1">
                    <p class="text-sm font-medium leading-none">
                        {{ order.orderBy.firstName }} {{ order.orderBy.lastName }}
                    </p>
                    <p class="text-sm text-muted-foreground">{{ order.orderBy.email }}</p>
                </div>
                <div class="ml-auto">
                    <span
                        :class="{
                            'bg-blue-500 text-white': order.status === 'pending',
                            'bg-green-500 text-white': order.status === 'accepted',
                            'bg-red-500 text-white': order.status === 'rejected',
                        }"
                        class="px-3 py-1 rounded-full text-sm font-medium"
                    >
                        {{ getStatusMessage(order.status) }}
                    </span>
                </div>
            </div>
        </div>
    </div>
</template>
