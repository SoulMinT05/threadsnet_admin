<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { BarChart } from '@/components/ui/chart-bar';

type Data = { year: number; total: number };
const dataPosts = ref<Data[]>([]);

const fetchData = async () => {
    try {
        const user = JSON.parse(localStorage.getItem('user'));
        const userToken = user.accessToken;
        const res = await fetch('http://localhost:3001/api/statistic/year', {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                Authorization: `Bearer ${userToken}`,
            },
        });
        const data = await res.json();
        console.log('dataChartYearrrrr: ', data);

        // Giả định rằng data.orders.populateComments là mảng các đơn hàng với thuộc tính createdAt
        // Khởi tạo mảng yearlyData với một số năm cần so sánh
        const yearlyData: Data[] = [];

        // Cập nhật số liệu từ API
        if (Array.isArray(data.posts.populatePosts)) {
            data.posts.populatePosts.forEach((post) => {
                const year = new Date(post.createdAt).getFullYear();
                const existingYearData = yearlyData.find((item) => item.year === year);

                if (existingYearData) {
                    existingYearData.total += 1; // Tăng tổng số đơn hàng theo năm
                } else {
                    yearlyData.push({ year, total: 1 }); // Thêm mới nếu chưa có
                }
            });
        }

        dataPosts.value = yearlyData; // Lưu dữ liệu vào biến reactive
    } catch (error) {
        console.error('Error fetching data:', error);
    }
};

onMounted(() => {
    fetchData(); // Gọi hàm fetchData khi component được mounted
});
</script>

<template>
    <BarChart :data="dataPosts" :categories="['total']" :index="'year'" :rounded-corners="0" />
</template>
