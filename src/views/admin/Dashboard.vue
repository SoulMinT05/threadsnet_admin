<script setup lang="ts">
import { Card, CardHeader, CardTitle, CardContent, CardDescription } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs';
import { DateRangePicker } from '@/components/ui/daterange-picker';
import StatisticWeek from './StatisticWeek.vue';
import StatisticMonth from './StatisticMonth.vue';
import StatisticYear from '@/views/admin/StatisticYear.vue';

import { ref, onMounted, computed } from 'vue';
import { useToast } from 'vue-toastification';

const statistics = ref({
    // Week
    currentWeek: {
        users: 0,
        posts: 0,
        comments: 0,
        conversations: 0,
    },
    growthRatesWeek: {
        users: 0,
        posts: 0,
        comments: 0,
        conversations: 0,
    },
    // Month
    currentMonth: {
        users: 0,
        posts: 0,
        comments: 0,
        conversations: 0,
    },
    growthRatesMonth: {
        users: 0,
        posts: 0,
        comments: 0,
        conversations: 0,
    },
    // Year
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
    statisticsWeek: [],
    statisticsMonth: [],
});

const fetchStatisticsWeek = async () => {
    try {
        const user = JSON.parse(localStorage.getItem('user'));
        const userToken = user.accessToken;
        const res = await fetch('http://localhost:3001/api/statistic/week', {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                Authorization: `Bearer ${userToken}`,
            },
        });
        const data = await res.json();
        const toast = useToast();
        console.log('dataWeek: ', data);

        if (!data.success) {
            toast.error(data.message);
            return;
        }
        statistics.value.statisticsWeek = data.statisticsWeek.map((weekData) => ({
            week: weekData.week,
            users: {
                title: weekData.users.title,
                count: weekData.users.count,
                growthRate: weekData.users.growthRate,
            },
            posts: {
                title: weekData.posts.title,
                count: weekData.posts.count,
                growthRate: weekData.posts.growthRate,
            },
            comments: {
                title: weekData.comments.title,
                count: weekData.comments.count,
                growthRate: weekData.comments.growthRate,
            },
            conversations: {
                title: weekData.conversations.title,
                count: weekData.conversations.count,
                growthRate: weekData.conversations.growthRate,
            },
        }));
        console.log('statistics.value.statisticsWeek: ', statistics.value.statisticsWeek);
    } catch (error) {
        console.error('Failed to fetch statistics:', error);
    }
};

const fetchStatisticsMonth = async () => {
    try {
        const user = JSON.parse(localStorage.getItem('user'));
        const userToken = user.accessToken;
        const res = await fetch('http://localhost:3001/api/statistic/month', {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                Authorization: `Bearer ${userToken}`,
            },
        });
        const data = await res.json();
        const toast = useToast();
        console.log('dataMonth: ', data);

        if (!data.success) {
            toast.error(data.message);
            return;
        }
        statistics.value.statisticsMonth = data.statisticsMonth.map((monthData) => ({
            month: monthData.month,
            users: {
                title: monthData.users.title,
                count: monthData.users.count,
                growthRate: monthData.users.growthRate,
            },
            posts: {
                title: monthData.posts.title,
                count: monthData.posts.count,
                growthRate: monthData.posts.growthRate,
            },
            comments: {
                title: monthData.comments.title,
                count: monthData.comments.count,
                growthRate: monthData.comments.growthRate,
            },
            conversations: {
                title: monthData.conversations.title,
                count: monthData.conversations.count,
                growthRate: monthData.conversations.growthRate,
            },
        }));
    } catch (error) {
        console.error('Failed to fetch statistics:', error);
    }
};

const fetchStatisticsYear = async () => {
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
        const toast = useToast();
        console.log('dataYear: ', data);

        if (!data.success) {
            toast.error(data.message);
            return;
        }
        statistics.value.currentYear = {
            users: data.users,
            posts: data.posts,
            comments: data.comments,
            conversations: data.conversations,
        };
        statistics.value.growthRatesYear = {
            users: data.users.growthRate,
            posts: data.posts.growthRate,
            comments: data.comments.growthRate,
            conversations: data.conversations.growthRate,
        };
    } catch (error) {
        console.error('Failed to fetch statistics:', error);
    }
};

onMounted(() => {
    fetchStatisticsWeek();
    fetchStatisticsMonth();
    fetchStatisticsYear();
});
</script>

<template>
    <div>
        <page-header title="Dashboard">
            <div class="flex items-center space-x-2">
                <DateRangePicker />
                <Button>Download</Button>
            </div>
        </page-header>

        <Tabs default-value="monthly" class="space-y-4">
            <TabsList>
                <TabsTrigger value="weekly">Tuần </TabsTrigger>
                <TabsTrigger value="monthly">Tháng </TabsTrigger>
                <TabsTrigger value="yearly">Năm </TabsTrigger>
            </TabsList>
            <TabsContent value="weekly" class="space-y-4">
                <StatisticWeek :statistics="statistics" />
            </TabsContent>
            <TabsContent value="monthly" class="space-y-4">
                <StatisticMonth :statistics="statistics" />
            </TabsContent>
            <TabsContent value="yearly" class="space-y-4">
                <StatisticYear :statistics="statistics" />
            </TabsContent>
        </Tabs>
    </div>
</template>
