<template>
    <div class="container">
        <h1 class="text-3xl font-bold mb-6">Quản lý bình luận</h1>
        <div class="flex justify-between items-center">
            <!-- <button
                @click="showAddPostModal"
                type="button"
                class="cursor-pointer text-white mb-4 bg-blue-500 hover:bg-blue-600 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center inline-flex items-center me-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800 transition duration-150 ease-in-out"
            >
                Thêm bình luận
            </button> -->
            <button
                @click="exportToExcel"
                class="cursor-pointer text-white mb-4 bg-green-500 hover:bg-green-600 focus:ring-4 focus:outline-none focus:ring-green-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center inline-flex items-center me-2 dark:bg-green-600 dark:hover:bg-green-700 dark:focus:ring-green-800 transition duration-150 ease-in-out"
            >
                Xuất excel
            </button>
        </div>

        <div class="mb-6">
            <input
                type="text"
                v-model="searchQuery"
                placeholder="Tìm kiếm bình luận..."
                class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
            />
        </div>

        <div class="overflow-x-auto bg-white shadow-md rounded-lg">
            <div v-if="isLoading" class="flex justify-center items-center py-10">
                <div class="loader"></div>
            </div>

            <table v-else class="w-full table-auto">
                <thead>
                    <tr class="text-gray-600 uppercase text-sm leading-normal">
                        <th class="py-3 px-6 text-left cursor-pointer" @click="sortBy('postId')">
                            Bài viết
                            <span v-if="currentSort !== 'postId'" class="ml-2">
                                <i class="fas fa-sort"></i>
                            </span>
                            <span v-if="currentSort === 'postId' && currentSortDir === 'asc'" class="ml-2">
                                <i class="fas fa-sort-up"></i>
                            </span>
                            <span v-if="currentSort === 'postId' && currentSortDir === 'desc'" class="ml-2">
                                <i class="fas fa-sort-down"></i>
                            </span>
                        </th>
                        <th class="py-3 px-6 text-left cursor-pointer" @click="sortBy('avatar')">
                            Ảnh đại diện
                            <span v-if="currentSort !== 'avatar'" class="ml-2">
                                <i class="fas fa-sort"></i>
                            </span>
                            <span v-if="currentSort === 'avatar' && currentSortDir === 'asc'" class="ml-2">
                                <i class="fas fa-sort-up"></i>
                            </span>
                            <span v-if="currentSort === 'avatar' && currentSortDir === 'desc'" class="ml-2">
                                <i class="fas fa-sort-down"></i>
                            </span>
                        </th>
                        <th class="py-3 px-6 text-left cursor-pointer" @click="sortBy('username')">
                            Username
                            <span v-if="currentSort !== 'username'" class="ml-2">
                                <i class="fas fa-sort"></i>
                            </span>
                            <span v-if="currentSort === 'username' && currentSortDir === 'asc'" class="ml-2">
                                <i class="fas fa-sort-up"></i>
                            </span>
                            <span v-if="currentSort === 'username' && currentSortDir === 'desc'" class="ml-2">
                                <i class="fas fa-sort-down"></i>
                            </span>
                        </th>
                        <th class="py-3 px-6 text-left cursor-pointer" @click="sortBy('textComment')">
                            Bình luận
                            <span v-if="currentSort !== 'textComment'" class="ml-2">
                                <i class="fas fa-sort"></i>
                            </span>
                            <span v-if="currentSort === 'textComment' && currentSortDir === 'asc'" class="ml-2">
                                <i class="fas fa-sort-up"></i>
                            </span>
                            <span v-if="currentSort === 'textComment' && currentSortDir === 'desc'" class="ml-2">
                                <i class="fas fa-sort-down"></i>
                            </span>
                        </th>

                        <th class="py-3 px-6 text-center cursor-pointer" @click="sortBy('status')">
                            Trạng thái
                            <span v-if="currentSort !== 'status'" class="ml-2">
                                <i class="fas fa-sort"></i>
                            </span>
                            <span v-if="currentSort === 'status' && currentSortDir === 'asc'" class="ml-2">
                                <i class="fas fa-sort-up"></i>
                            </span>
                            <span v-if="currentSort === 'status' && currentSortDir === 'desc'" class="ml-2">
                                <i class="fas fa-sort-down"></i>
                            </span>
                        </th>
                        <th class="py-3 px-6 text-center cursor-pointer" @click="sortBy('createdAt')">
                            Ngày tạo
                            <span v-if="currentSort !== 'createdAt'" class="ml-2">
                                <i class="fas fa-sort"></i>
                            </span>
                            <span v-if="currentSort === 'createdAt' && currentSortDir === 'asc'" class="ml-2">
                                <i class="fas fa-sort-up"></i>
                            </span>
                            <span v-if="currentSort === 'createdAt' && currentSortDir === 'desc'" class="ml-2">
                                <i class="fas fa-sort-down"></i>
                            </span>
                        </th>
                        <!-- <th class="py-3 px-6 text-center">
                            Ngày cập nhật
                            <span v-if="currentSort !== 'updatedAt'" class="ml-2">
                                <i class="fas fa-sort"></i>
                            </span>
                            <span v-if="currentSort === 'updatedAt' && currentSortDir === 'asc'" class="ml-2">
                                <i class="fas fa-sort-up"></i>
                            </span>
                            <span v-if="currentSort === 'updatedAt' && currentSortDir === 'desc'" class="ml-2">
                                <i class="fas fa-sort-down"></i>
                            </span>
                        </th> -->

                        <th class="py-3 px-6 text-center">Hành động</th>
                    </tr>
                </thead>
                <tbody class="text-gray-600 text-sm font-light">
                    <tr
                        v-for="comment in sortedAndPaginatedComments"
                        :key="comment?.id"
                        class="border-b border-gray-200 hover:bg-gray-100"
                    >
                        <td class="py-4 px-6 text-left">
                            <span class="font-medium">
                                {{ truncateText(comment?.postId?.text, 20) }}
                            </span>
                        </td>

                        <td class="py-4 px-6 text-left">
                            <img
                                :src="comment?.avatar"
                                :style="{
                                    width: '76px',
                                    height: '76px',
                                    objectFit: 'cover',
                                    borderRadius: '8px',
                                }"
                                alt=""
                            />
                        </td>

                        <td class="py-4 px-6 text-left">
                            <span class="font-medium">{{ comment?.username }}</span>
                        </td>
                        <td class="py-4 px-6 text-left">
                            <span class="font-medium truncate">{{ truncateText(comment?.textComment, 20) }}</span>
                        </td>

                        <td class="py-4 px-6 text-left">
                            <Badge
                                v-if="comment?.status === false"
                                class="bg-green-200 text-green-600 py-1 px-3 rounded-full text-xs"
                                >Tích cực</Badge
                            >

                            <Badge
                                v-else-if="comment?.status === true"
                                class="bg-red-200 text-red-600 py-1 px-3 rounded-full text-xs"
                                >Nhạy cảm</Badge
                            >
                        </td>
                        <td class="py-4 px-6 text-center">
                            <span>{{ formatDate(comment?.createdAt) }}</span>
                        </td>
                        <!-- <td class="py-4 px-6 text-center">
                            <span>{{ formatDate(comment?.updatedAt) }}</span>
                        </td> -->
                        <td class="py-4 px-6 text-center">
                            <div class="flex item-center justify-center">
                                <button
                                    @click="viewComment(comment)"
                                    class="cursor-pointer hover:opacity-95 w-4 mr-2 transform hover:text-purple-500 hover:scale-110 transition-all duration-300 z-50"
                                >
                                    <i class="fas fa-eye"></i>
                                </button>
                                <!-- <button
                                    @click="editcomment(comment)"
                                    class="cursor-pointer hover:opacity-95 w-4 mr-2 transform hover:text-yellow-500 hover:scale-110 transition-all duration-300"
                                >
                                    <i class="fas fa-edit"></i>
                                </button> -->
                                <button
                                    @click="deleteComment(comment)"
                                    class="cursor-pointer hover:opacity-95 w-4 mr-2 transform hover:text-red-500 hover:scale-110 transition-all duration-300"
                                >
                                    <i class="fas fa-trash-alt"></i>
                                </button>
                            </div>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <!-- View detail product -->
        <div
            v-if="selectedComment"
            class="fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center z-50 overflow-y-scroll"
        >
            <div @click.stop class="bg-white rounded-lg shadow-lg p-6 w-11/12 md:w-2/3 lg:w-1/2">
                <h2 class="text-xl font-bold text-center mb-4">Thông tin bình luận</h2>

                <div class="space-y-2">
                    <p><strong>Ảnh đại diện:</strong></p>
                    <div class="flex flex-wrap justify-start gap-3 mb-4">
                        <img
                            :src="selectedComment.avatar"
                            alt="Ảnh đại diện"
                            class="rounded-lg w-24 h-24 object-cover"
                        />
                    </div>
                    <p><strong>Người bình luận: </strong> {{ selectedComment.username }}</p>
                    <p><strong>Bài viết: </strong> {{ selectedComment?.postId?.text }}</p>
                    <p><strong>Bình luận: </strong> {{ selectedComment.textComment }}</p>
                    <p><strong>Lượt thích:</strong> {{ selectedComment.likes.length }}</p>
                    <p><strong>Phản hồi bình luận:</strong> {{ selectedComment.replies.length }}</p>
                    <p>
                        <strong>Chế độ:</strong>
                        <span
                            v-if="selectedComment.status === false"
                            class="bg-green-200 text-green-600 py-1 px-3 rounded-full text-xs"
                        >
                            Tích cực
                        </span>
                        <span
                            v-else-if="selectedComment.status === true"
                            class="bg-red-200 text-red-600 py-1 px-3 rounded-full text-xs"
                        >
                            Nhạy cảm
                        </span>
                    </p>

                    <p><strong>Ngày tạo:</strong> {{ formatDate(selectedComment.createdAt) }}</p>
                    <p><strong>Ngày cập nhật:</strong> {{ formatDate(selectedComment.updatedAt) }}</p>
                </div>
                <div class="mt-6 flex justify-end">
                    <button
                        @click="closeModal"
                        class="cursor-pointer hover:opacity-95 text-white bg-blue-600 rounded-md px-4 py-2 transition duration-300 ease-in-out transform hover:scale-105"
                    >
                        Đóng
                    </button>
                </div>
            </div>
        </div>

        <div class="mt-4 flex justify-between">
            <button
                @click="goToPage(currentPage - 1)"
                :disabled="currentPage === 1"
                class="px-4 py-2 rounded-md cursor-pointer hover:opacity-95 transition duration-150 ease-in-out"
                :class="{
                    'bg-blue-500 text-white': currentPage > 1,
                    'bg-gray-400 text-gray-200 cursor-not-allowed': currentPage === 1,
                }"
            >
                Trước
            </button>
            <span class="self-center">Trang {{ currentPage }}/{{ totalPages }}</span>
            <button
                @click="goToPage(currentPage + 1)"
                :disabled="currentPage === totalPages"
                class="px-4 py-2 rounded-md cursor-pointer hover:opacity-95 transition duration-150 ease-in-out"
                :class="{
                    'bg-blue-500 text-white': currentPage < totalPages,
                    'bg-gray-400 text-gray-200 cursor-not-allowed': currentPage === totalPages,
                }"
            >
                Sau
            </button>
        </div>
    </div>
</template>

<script>
import { useToast } from 'vue-toastification';
import * as XLSX from 'xlsx';

export default {
    data() {
        return {
            comments: [], // Dữ liệu bình luận
            currentSort: '', // Cột hiện tại để sắp xếp
            currentSortDir: 'asc',
            searchQuery: '',
            newPost: {
                name: '',
                author: '',
                price: null,
                quantity: null,
                yearOfPublication: null,
                publisher: '',
            },
            // Create images
            previewImage: null,
            uploadImages: [],
            previewImages: [],
            // Edit images
            currentImages: [], // Ảnh hiện tại của sản phẩm
            previewNewImages: [], // Preview ảnh mới
            newImages: [], // Danh bình luận file ảnh mới
            deletedImages: [], // Danh bình luận ảnh bị xóa
            publishers: [],
            isAddPostModalVisible: false,
            loading: false,
            loadingEdit: false,
            currentPage: 1, // Bắt đầu với trang đầu tiên
            pageSize: 10, // Hiển thị 10 bình luận mỗi trang
            selectedComment: null,
            postToEdit: null,
            isEditPostModalVisible: false, // For Edit product modal
            isLoading: false,
        };
    },
    computed: {
        totalPages() {
            return Math.ceil(this.filteredComments.length / this.pageSize);
        },
        paginatedPosts() {
            const start = (this.currentPage - 1) * this.pageSize;
            return this.filteredComments.slice(start, start + this.pageSize);
        },
        sortedAndPaginatedComments() {
            // Sắp xếp dữ liệu
            const sortedComments = this.sortedComments;

            // Tính toán các chỉ số phân trang
            const start = (this.currentPage - 1) * this.pageSize;
            const end = start + this.pageSize;

            // Trả về dữ liệu phân trang sau khi đã sắp xếp
            return sortedComments.slice(start, end);
        },
        sortedComments() {
            // If no sort column is specified, return the Posts array as is
            const commentsToSort = this.filteredComments;
            if (!this.currentSort) {
                return commentsToSort;
            }

            const direction = this.currentSortDir === 'asc' ? 1 : -1;
            return [...commentsToSort].sort((a, b) => {
                let valueA = a[this.currentSort];
                let valueB = b[this.currentSort];

                if (valueA < valueB) return -1 * direction;
                if (valueA > valueB) return 1 * direction;
                return 0;
            });
        },
        filteredComments() {
            const searchQuery = this.searchQuery.toLowerCase().replace(/\//g, '');
            return this.comments.filter((comment) => {
                const username = comment.username ? comment.username.toLowerCase() : '';
                const postId = comment?.postId?.text ? comment?.postId?.text.toLowerCase() : '';
                const textComment = comment.textComment ? comment.textComment.toLowerCase() : '';
                // const publisher = comment.publisherId?.name ? comment.publisherId?.name.toLowerCase() : '';
                const createdAt = comment.createdAt ? new Date(comment.createdAt) : null;
                const updatedAt = comment.updatedAt ? new Date(comment.updatedAt) : null;

                const formattedCreatedAt = createdAt
                    ? createdAt.getDate().toString().padStart(2, '0') +
                      (createdAt.getMonth() + 1).toString().padStart(2, '0') +
                      createdAt.getFullYear()
                    : '';
                const formattedUpdatedAt = updatedAt
                    ? updatedAt.getDate().toString().padStart(2, '0') +
                      (updatedAt.getMonth() + 1).toString().padStart(2, '0') +
                      updatedAt.getFullYear()
                    : '';

                const searchQueryNumber = Number(searchQuery);
                const isSearchQueryNumber = !isNaN(searchQueryNumber);

                const matchNumber = (value) => {
                    if (isSearchQueryNumber && value != null) {
                        return value.toString().includes(searchQuery); // Tìm kiếm theo chuỗi số
                    }
                    return false;
                };

                return (
                    username.includes(searchQuery.toLowerCase()) ||
                    postId.includes(searchQuery.toLowerCase()) ||
                    textComment.includes(searchQuery.toLowerCase()) ||
                    publisher.includes(searchQuery.toLowerCase()) ||
                    matchNumber(post.numberViews) ||
                    visibility.includes(searchQuery.toLowerCase()) ||
                    formattedCreatedAt.includes(searchQuery) || // So sánh chuỗi ngày tháng
                    formattedUpdatedAt.includes(searchQuery)
                );
            });
        },
    },
    methods: {
        truncateText(text, maxLength) {
            if (!text) return '';
            return text.length > maxLength ? text.substring(0, maxLength) + '...' : text;
        },
        sortBy(column) {
            if (this.currentSort === column) {
                // Nếu đã sắp xếp theo cột này, đổi chiều
                this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc';
            } else {
                // Nếu chưa sắp xếp theo cột này, sắp xếp tăng dần
                this.currentSort = column;
                this.currentSortDir = 'asc';
            }
        },
        nextPage() {
            if (this.currentPage < this.totalPages) {
                this.currentPage++;
            }
        },
        previousPage() {
            if (this.currentPage > 1) {
                this.currentPage--;
            }
        },
        goToPage(page) {
            this.currentPage = page;
        },
        resetToFirstPage() {
            this.currentPage = 1;
        },
        validateNumber(event) {
            const inputValue = event.target.value;
            // Chỉ cho phép nhập số hoặc chuỗi rỗng
            if (!/^\d*$/.test(inputValue)) {
                event.target.value = inputValue.replace(/[^\d]/g, ''); // Loại bỏ ký tự không phải số
            }
        },
        formatDate(date) {
            // const createdAtDate = new Date(date); // Tạo đối tượng Date
            // return createdAtDate.toGMTString(); // Chuyển đổi thành chuỗi GMT
            const d = new Date(date);
            const year = d.getUTCFullYear();
            const month = String(d.getUTCMonth() + 1).padStart(2, '0');
            const day = String(d.getUTCDate()).padStart(2, '0');

            return `${day}/${month}/${year}`;
        },
        formatPrice(value) {
            if (value >= 1000000) {
                return (value / 1000000).toFixed(1).replace(/\.0$/, '') + 'M VND';
            } else if (value >= 1000) {
                return (value / 1000).toFixed(1).replace(/\.0$/, '') + 'k VND';
            } else {
                return value.toString();
            }
        },
        exportToExcel() {
            // Tạo một workbook mới
            const wb = XLSX.utils.book_new();

            // Chuyển đổi dữ liệu bình luận thành bảng
            const ws = XLSX.utils.json_to_sheet(this.comments);

            // Thêm bảng vào workbook
            XLSX.utils.book_append_sheet(wb, ws, 'comments');

            // Tạo tên file
            const fileName = `comments_${new Date().toISOString().split('T')[0]}.xlsx`;

            // Xuất file
            XLSX.writeFile(wb, fileName);
        },
        exportToPDF() {
            // Tạo một tài liệu PDF mới
            const doc = new jsPDF();

            // Tiêu đề
            doc.setFontSize(20);
            doc.text('Danh bình luận sản phẩm', 10, 10);

            // Thêm tiêu đề bảng
            doc.setFontSize(12);
            const headers = ['Tên', 'Tác giả', 'Giá', 'Số lượng', 'Năm xuất bản', 'Nhà xuất bản'];
            const data = this.comments.map((comment) => [
                comment.username,
                comment.postId.name,
                comment.textComment,
                this.formatPrice(comment.price),
                comment.quantity,
                comment.yearOfPublication,
                comment.publisher,
            ]);

            // Tạo bảng
            doc.autoTable({
                head: [headers],
                body: data,
                startY: 20, // Bắt đầu vẽ bảng từ vị trí Y = 20
            });

            // Lưu file PDF
            doc.save(`products_${new Date().toISOString().split('T')[0]}.pdf`);
        },
        // async fetchPublishers() {
        //     this.isLoading = true;
        //     try {
        //         const user = JSON.parse(localStorage.getItem('user'));
        //         const userToken = user.accessToken;
        //         const res = await fetch('http://localhost:3001/api/publisher/getAllPublishers', {
        //             method: 'GET',
        //             headers: {
        //                 'Content-Type': 'application/json',
        //                 Authorization: `Bearer ${userToken}`,
        //             },
        //         });
        //         const data = await res.json();
        //         const toast = useToast();
        //         console.log('dataPublishers: ', data);

        //         if (!data.success) {
        //             toast.error(data.message);
        //             return;
        //         }
        //         this.publishers = data.publishers;
        //     } catch (error) {
        //         console.log('error: ', error);
        //         toast.error(error.message);
        //     } finally {
        //         this.isLoading = false;
        //     }
        // },
        async fetchComments() {
            this.isLoading = true;
            try {
                const user = JSON.parse(localStorage.getItem('user'));
                const userToken = user.accessToken;
                const res = await fetch('http://localhost:3001/api/comment/getAllCommentsFromAdmin', {
                    method: 'GET',
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `Bearer ${userToken}`,
                    },
                });
                const data = await res.json();
                const toast = useToast();
                console.log('dataComments: ', data);

                if (!data.success) {
                    toast.error(data.message);
                    return;
                }
                this.comments = data.comments;
            } catch (error) {
                console.log('error: ', error);
                toast.error(error.message);
            } finally {
                this.isLoading = false;
            }
        },
        showAddPostModal() {
            this.isAddPostModalVisible = true;
        },
        closeAddPostModal() {
            this.isAddPostModalVisible = false;
            this.resetNewPost(); // Đặt lại thông tin bình luận mới sau khi đóng modal
        },

        handleFileUpload(event) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    this.previewImage = e.target.result; // Lưu URL base64 để hiển thị hình ảnh
                };
                reader.readAsDataURL(file); // Đọc tệp ảnh
            }
        },

        handleFileChange(event) {
            const files = event.target.files; // Lấy danh bình luận file từ input
            if (files.length > 10) {
                // Nếu người dùng chọn quá 10 ảnh, có thể báo lỗi
                alert('Chọn tối đa 10 ảnh.');
                return;
            }

            this.uploadImages = []; // Xóa danh bình luận ảnh cũ
            this.previewImages = [];
            for (let i = 0; i < files.length; i++) {
                this.uploadImages.push(files[i]); // Thêm từng file vào uploadImages

                // Tạo URL cho preview
                const reader = new FileReader();
                reader.onload = (e) => {
                    this.previewImages.push(e.target.result); // Lưu URL của ảnh
                };
                reader.readAsDataURL(files[i]); // Đọc file dưới dạng UR
            }

            console.log('this.uploadImages: ', this.uploadImages);
            console.log('this.previewImages: ', this.previewImages);
        },

        // Edit product
        handleNewFileChange(event) {
            const files = event.target.files;
            this.previewNewImages = [...files].map((file) => URL.createObjectURL(file));
            this.newImages = Array.from(files);
            console.log('filesEditImgNew: ', files);
        },
        async removeCurrentImage(index) {
            if (!window.confirm('Bạn chắc chắn xoá ảnh này chứ?')) return;
            const deletedImage = this.currentImages[index]; // Lấy ảnh từ currentImages
            this.deletedImages.push(deletedImage); // Thêm ảnh vào mảng deletedImages

            // Xóa ảnh khỏi currentImages (chỉ xóa trên giao diện)
            this.currentImages.splice(index, 1);
        },

        editPost(post) {
            this.postToEdit = { ...post }; // Make a copy of the publisher object to avoid directly modifying the array
            this.currentImages = [...post.images];
            this.deletedImages = [];
            this.newImages = [];
            this.isEditPostModalVisible = true;
        },
        closeEditProductModal() {
            this.isEditPostModalVisible = false;
            this.postToEdit = null; // Clear the postToEdit data after closing the modal
            this.deletedImages = [];
            this.newImages = [];
            this.currentImages = [];
            this.previewNewImages = [];
        },
        async saveEditedProduct() {
            this.loading = true;
            try {
                const user = JSON.parse(localStorage.getItem('user'));
                const userToken = user.accessToken;

                // Lấy productId từ postToEdit
                const productId = this.postToEdit._id;

                const formData = new FormData();
                // Gán các thông tin sản phẩm từ postToEdit vào FormData
                formData.append('name', this.postToEdit.name);
                formData.append('author', this.postToEdit.author);
                formData.append('price', this.postToEdit.price);
                formData.append('quantity', this.postToEdit.quantity);
                formData.append('yearOfPublication', this.postToEdit.yearOfPublication);
                formData.append('publisher', this.postToEdit.publisher);
                // Gửi đường dẫn hình ảnh đã xóa
                if (this.deletedImages.length > 0) {
                    formData.append('deleteImage', JSON.stringify(this.deletedImages));
                }
                // Thêm hình ảnh mới (là các file, không phải link text)
                if (this.newImages.length > 0) {
                    for (const file of this.newImages) {
                        formData.append('images', file); // Chỉ thêm file, không thêm link
                    }
                }

                const res = await fetch(`http://localhost:3001/api/book/${productId}`, {
                    method: 'PUT',
                    headers: {
                        Authorization: `Bearer ${userToken}`,
                    },
                    body: formData, // Gửi FormData
                });

                const data = await res.json();
                const toast = useToast();
                console.log('dataEditProducts: ', data);

                if (!data.success) {
                    toast.error(data.message);
                    return;
                }

                // Cập nhật danh bình luận sản phẩm sau khi chỉnh sửa
                const index = this.products.findIndex((product) => product._id === productId);
                if (index !== -1) {
                    this.products[index] = data.updatedProduct;
                }

                toast.success('Cập nhật thông tin bình luận thành công');
                this.closeEditProductModal();
            } catch (error) {
                console.error('Error adding product:', error);
                const toast = useToast();
                toast.error(error.message);
            } finally {
                this.loading = false;
            }
        },

        viewComment(comment) {
            // Logic xem chi tiết bình luận
            this.selectedComment = comment;
            console.log('this.selectedComment: ', this.selectedComment);
        },
        closeModal() {
            this.selectedComment = null;
        },

        async deleteComment(comment) {
            if (!confirm('Bạn chắc chắn xoá bình luận này không?')) return;
            try {
                const userLocalStorage = JSON.parse(localStorage.getItem('user'));
                const userToken = userLocalStorage.accessToken;

                const res = await fetch(`http://localhost:3001/api/comment/deleteCommentFromAdmin/${comment._id}`, {
                    method: 'DELETE',
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `Bearer ${userToken}`,
                    },
                });

                const data = await res.json();
                const toast = useToast();
                console.log('dataDeleteComment: ', data);

                if (!data.success) {
                    toast.error(data.message);
                    return;
                }
                this.comments.splice(
                    this.comments.findIndex((c) => c._id === comment._id),
                    1,
                );
                toast.success('Xoá bình luận thành công');
            } catch (error) {
                console.error('Error deleting user:', error.message);
                toast.error('Error deleting user: ', error.message);
            }
        },

        showToast(message) {
            this.toastMessage = message;
            setTimeout(() => {
                this.toastMessage = '';
            }, 3000);
        },
    },
    watch: {
        searchQuery() {
            this.resetToFirstPage();
        },
    },

    mounted() {
        this.fetchComments();
        // this.fetchPublishers();
    },
};
</script>

<style scoped>
/* Thêm CSS tùy chỉnh ở đây nếu cần */
.loader {
    border: 4px solid rgba(0, 0, 0, 0.1);
    border-left-color: #4f46e5;
    border-radius: 50%;
    width: 20px; /* Giảm kích thước của loader */
    height: 20px;
    animation: spin 1s linear infinite;
}
.modal-content {
    max-height: 100vh; /* Đặt chiều cao tối đa cho modal */
    overflow-y: auto; /* Cho phép cuộn nếu nội dung vượt quá chiều cao */
}
.tooltip {
    position: relative;
    display: inline-block;
}

.tooltip .tooltiptext {
    visibility: hidden;
    width: 60px;
    background-color: black;
    color: #fff;
    text-align: center;
    border-radius: 6px;
    padding: 5px 0;
    position: absolute;
    z-index: 1;
    bottom: 125%; /* Xuất hiện ở phía trên biểu tượng */
    left: 50%;
    margin-left: -30px; /* Trung tâm tooltip */
    opacity: 0;
    transition: opacity 0.3s;
}

.tooltip:hover .tooltiptext {
    visibility: visible;
    opacity: 1;
}

@keyframes spin {
    to {
        transform: rotate(360deg);
    }
}

span.truncate {
    display: block;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    max-width: 300px; /* Điều chỉnh theo yêu cầu */
}
</style>
