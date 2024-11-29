<template>
    <div class="flex flex-col items-center space-y-4 py-4">
        <!-- Thêm từ nhạy cảm -->
        <div class="w-full max-w-md">
            <Card>
                <CardHeader>
                    <CardTitle>Thêm Từ Nhạy Cảm</CardTitle>
                    <CardDescription
                        >Nhập từ bạn muốn thêm vào danh sách từ nhạy cảm. Các từ cách nhau bởi dấu
                        phẩy.</CardDescription
                    >
                </CardHeader>
                <CardContent>
                    <Input
                        v-model="newWord"
                        placeholder="Nhập từ nhạy cảm (cách nhau bởi dấu phẩy)"
                        class="w-full"
                        :disabled="loading"
                    />
                </CardContent>
                <CardFooter>
                    <Button :disabled="loading || !newWord" @click="handleAddSensitiveWords" color="primary">
                        {{ loading ? 'Đang thêm...' : 'Thêm từ' }}
                    </Button>
                </CardFooter>
            </Card>
        </div>

        <!-- Danh sách từ nhạy cảm -->
        <div class="w-full max-w-md">
            <Card>
                <CardHeader>
                    <CardTitle>Danh Sách Từ Nhạy Cảm</CardTitle>
                </CardHeader>
                <CardContent>
                    <ul class="space-y-2">
                        <li
                            v-for="(word, index) in sensitiveWords"
                            :key="index"
                            class="flex justify-between items-center"
                        >
                            <span>{{ word }}</span>
                            <Button color="danger" @click="handleRemoveSensitiveWord(word)" size="icon"> Xóa </Button>
                        </li>
                    </ul>
                </CardContent>
            </Card>
        </div>

        <!-- Thông báo -->
        <!-- <Alert v-if="message" :type="messageType" class="w-full max-w-md">
      <p>{{ message }}</p>
    </Alert> -->
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { Card, CardHeader, CardContent, CardFooter, CardTitle, CardDescription } from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { Button } from '@/components/ui/button';
// import { Alert } from '@/components/ui/alert';
import { useToast } from 'vue-toastification';

const toast = useToast();
const newWord = ref('');
const sensitiveWords = ref([]);
const message = ref('');
const messageType = ref('success');
const loading = ref(false);

const fetchSensitiveWords = async () => {
    try {
        const user = JSON.parse(localStorage.getItem('user') || '{}'); // Xử lý khi không có user
        const userToken = user.accessToken;

        const response = await fetch('/api/sensitiveWord/getSensitiveWords', {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                Authorization: `Bearer ${userToken}`,
            },
        }); // API lấy danh sách từ nhạy cảm
        const data = await response.json();
        console.log('dataWords: ', data);
        if (data.success) {
            sensitiveWords.value = data.words;
        }
    } catch (error) {
        console.error('Error fetching words:', error);
    }
};

const handleAddSensitiveWords = async () => {
    loading.value = true;
    try {
        const user = JSON.parse(localStorage.getItem('user') || '{}'); // Xử lý khi không có user
        const userToken = user.accessToken;

        const words = newWord.value
            .split(',')
            .map((word) => word.trim().toLowerCase()) // Loại bỏ khoảng trắng thừa và chuyển thành chữ thường
            .filter((word) => word.length > 0);
        if (words.length === 0) {
            toast.error('Vui lòng nhập ít nhất một từ');
            return;
        }

        const response = await fetch('/api/sensitiveWord/addSensitiveWords', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                Authorization: `Bearer ${userToken}`,
            },
            body: JSON.stringify({ words }),
        });
        const data = await response.json();
        console.log('dataAddWords: ', data);
        if (data.success) {
            toast.success('Từ đã được thêm thành công!');
            newWord.value = '';
            await fetchSensitiveWords(); // Cập nhật danh sách
        } else {
            toast.error('Lỗi khi thêm từ!');
        }
    } catch (error) {
        toast.error('Lỗi khi thêm từ!');
    } finally {
        loading.value = false;
    }
};

const handleRemoveSensitiveWord = async (word) => {
    try {
        if (!window.confirm('Bạn chắc chắn xoá các từ nhạy cảm này?')) return;
        const user = JSON.parse(localStorage.getItem('user') || '{}'); // Xử lý khi không có user
        const userToken = user.accessToken;
        const response = await fetch('/api/sensitiveWord/deleteSensitiveWord', {
            method: 'DELETE',
            headers: {
                'Content-Type': 'application/json',
                Authorization: `Bearer ${userToken}`,
            },
            body: JSON.stringify({ word }),
        });
        const data = await response.json();
        console.log('dataRemoveWords: ', data);
        if (data.success) {
            // message.value = `Từ '${word}' đã bị xóa thành công!`;
            // messageType.value = 'success';
            toast.success(`Từ '${word}' đã bị xóa thành công!`);
            await fetchSensitiveWords(); // Cập nhật danh sách
        } else {
            // message.value = 'Lỗi khi xóa từ.';
            // messageType.value = 'error';
            toast.error('Lỗi khi xóa từ.');
        }
    } catch (error) {
        // message.value = 'Lỗi khi xóa từ.';
        // messageType.value = 'error';
        toast.error('Lỗi khi xóa từ.');
    }
};

fetchSensitiveWords(); // Lấy danh sách từ nhạy cảm khi trang được tải
</script>

<style scoped>
/* Có thể thêm style tùy theo yêu cầu */
</style>
