<template>
    <div class="container">
        <h1 class="text-3xl font-bold mb-6">Quản lý bài viết</h1>
        <div class="flex justify-between items-center">
            <!-- <button
                @click="showAddPostModal"
                type="button"
                class="cursor-pointer text-white mb-4 bg-blue-500 hover:bg-blue-600 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center inline-flex items-center me-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800 transition duration-150 ease-in-out"
            >
                Thêm bài viết
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
                placeholder="Tìm kiếm bài viết..."
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
                        <th class="py-3 px-6 text-left cursor-pointer">Hình ảnh</th>
                        <th class="py-3 px-6 text-left cursor-pointer" @click="sortBy('postedBy')">
                            Người đăng
                            <span v-if="currentSort !== 'postedBy'" class="ml-2">
                                <i class="fas fa-sort"></i>
                            </span>
                            <span v-if="currentSort === 'postedBy' && currentSortDir === 'asc'" class="ml-2">
                                <i class="fas fa-sort-up"></i>
                            </span>
                            <span v-if="currentSort === 'postedBy' && currentSortDir === 'desc'" class="ml-2">
                                <i class="fas fa-sort-down"></i>
                            </span>
                        </th>
                        <th class="py-3 px-6 text-left cursor-pointer" @click="sortBy('text')">
                            Bài viết
                            <span v-if="currentSort !== 'text'" class="ml-2">
                                <i class="fas fa-sort"></i>
                            </span>
                            <span v-if="currentSort === 'text' && currentSortDir === 'asc'" class="ml-2">
                                <i class="fas fa-sort-up"></i>
                            </span>
                            <span v-if="currentSort === 'text' && currentSortDir === 'desc'" class="ml-2">
                                <i class="fas fa-sort-down"></i>
                            </span>
                        </th>
                        <th class="py-3 px-6 text-left cursor-pointer" @click="sortBy('numberViews')">
                            Người xem
                            <span v-if="currentSort !== 'numberViews'" class="ml-2">
                                <i class="fas fa-sort"></i>
                            </span>
                            <span v-if="currentSort === 'numberViews' && currentSortDir === 'asc'" class="ml-2">
                                <i class="fas fa-sort-up"></i>
                            </span>
                            <span v-if="currentSort === 'numberViews' && currentSortDir === 'desc'" class="ml-2">
                                <i class="fas fa-sort-down"></i>
                            </span>
                        </th>
                        <th class="py-3 px-6 text-left cursor-pointer" @click="sortBy('visibility')">
                            Chế độ
                            <span v-if="currentSort !== 'visibility'" class="ml-2">
                                <i class="fas fa-sort"></i>
                            </span>
                            <span v-if="currentSort === 'visibility' && currentSortDir === 'asc'" class="ml-2">
                                <i class="fas fa-sort-up"></i>
                            </span>
                            <span v-if="currentSort === 'visibility' && currentSortDir === 'desc'" class="ml-2">
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
                        v-for="post in sortedAndPaginatedPosts"
                        :key="post?.id"
                        class="border-b border-gray-200 hover:bg-gray-100"
                    >
                        <!-- <td class="py-4 px-6 text-left">
                            <div v-if="post.images?.length">
                                <img class="w-16 h-16 object-cover" :src="post.images[0]" alt="post Image" />
                            </div>
                            <div v-else>No images available</div>
                        </td> -->
                        <td class="py-4 px-6 text-left">
                            <img
                                :src="post?.image"
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
                            <span class="font-medium">{{ post?.postedBy?.username }}</span>
                        </td>
                        <td class="py-4 px-6 text-left">
                            <span class="font-medium truncate">{{ truncateText(post?.text, 20) }}</span>
                        </td>
                        <td class="py-4 px-6 text-left">
                            <span class="font-medium">{{ post?.numberViews }}</span>
                        </td>
                        <td class="py-4 px-6 text-left">
                            <Badge
                                v-if="post?.visibility === 'public'"
                                class="bg-blue-200 text-blue-600 py-1 px-3 rounded-full text-xs"
                                >Công khai</Badge
                            >

                            <Badge
                                v-else-if="post?.visibility === 'friends'"
                                class="bg-yellow-200 text-yellow-600 py-1 px-3 rounded-full text-xs"
                                >Bạn bè</Badge
                            >
                            <Badge
                                v-if="post?.visibility === 'followers'"
                                class="bg-green-200 text-green-600 py-1 px-3 rounded-full text-xs"
                                >Người theo dõi</Badge
                            >

                            <Badge
                                v-else-if="post?.visibility === 'private'"
                                class="bg-gray-200 text-gray-600 py-1 px-3 rounded-full text-xs"
                                >Chỉ mình tôi</Badge
                            >
                        </td>
                        <td class="py-4 px-6 text-left">
                            <Badge
                                v-if="post?.status === false"
                                class="bg-green-200 text-green-600 py-1 px-3 rounded-full text-xs"
                                >Tích cực</Badge
                            >

                            <Badge
                                v-else-if="post?.status === true"
                                class="bg-red-200 text-red-600 py-1 px-3 rounded-full text-xs"
                                >Nhạy cảm</Badge
                            >
                        </td>
                        <td class="py-4 px-6 text-center">
                            <span>{{ formatDate(post?.createdAt) }}</span>
                        </td>
                        <!-- <td class="py-4 px-6 text-center">
                            <span>{{ formatDate(post?.updatedAt) }}</span>
                        </td> -->
                        <td class="py-4 px-6 text-center">
                            <div class="flex item-center justify-center">
                                <button
                                    @click="viewPost(post)"
                                    class="cursor-pointer hover:opacity-95 w-4 mr-2 transform hover:text-purple-500 hover:scale-110 transition-all duration-300 z-50"
                                >
                                    <i class="fas fa-eye"></i>
                                </button>
                                <!-- <button
                                    @click="editPost(post)"
                                    class="cursor-pointer hover:opacity-95 w-4 mr-2 transform hover:text-yellow-500 hover:scale-110 transition-all duration-300"
                                >
                                    <i class="fas fa-edit"></i>
                                </button> -->
                                <button
                                    @click="deletePost(post)"
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

        <!-- Add post modal -->
        <div
            v-if="isAddPostModalVisible"
            class="fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center z-50"
        >
            <div class="bg-white rounded-lg p-6 w-11/12 md:w-2/3 lg:w-1/2 modal-content">
                <h2 class="text-lg font-bold mb-4">Thêm bài viết</h2>
                <form @submit.prevent="addPost">
                    <label for="image" class="block text-sm font-medium text-gray-700"> Hình ảnh </label>
                    <input
                        type="file"
                        id="image"
                        accept="image/*"
                        @change="handleFileUpload"
                        class="mt-1 p-2 w-full border border-gray-300 rounded-md"
                    />

                    <!-- Hiển thị hình ảnh -->
                    <div v-if="previewImage" class="mt-4">
                        <p class="text-sm text-gray-500">Xem trước hình ảnh:</p>
                        <img
                            :src="previewImage"
                            alt="Preview"
                            class="mt-1 p-2 w-full border border-gray-300 rounded-md"
                        />
                    </div>
                    <div class="mb-4">
                        <label for="text" class="block text-sm font-medium text-gray-700">Người đăng</label>
                        <input
                            v-model="newPost.postedBy"
                            type="text"
                            id="text"
                            class="mt-1 p-2 w-full border border-gray-300 rounded-md"
                            required
                            disabled
                        />
                    </div>
                    <div class="mb-4">
                        <label for="text" class="block text-sm font-medium text-gray-700">Nội dung bài viết</label>
                        <input
                            v-model="newPost.text"
                            type="text"
                            id="text"
                            class="mt-1 p-2 w-full border border-gray-300 rounded-md"
                            required
                        />
                    </div>
                    <div class="mb-4">
                        <label for="visibility" class="block text-sm font-medium text-gray-700">
                            Chế độ bài viết
                        </label>
                        <select
                            v-model="newPost.visibility"
                            id="visibility"
                            class="mt-1 p-2 w-full border border-gray-300 rounded-md"
                        >
                            <option value="public">Công khai</option>
                            <option value="friends">Bạn bè</option>
                            <option value="followers">Người theo dõi</option>
                            <option value="private">Chỉ mình tôi</option>
                        </select>
                    </div>

                    <div class="flex justify-end mt-6">
                        <button
                            type="button"
                            @click="closeAddPostModal"
                            class="cursor-pointer hover:opacity-95 mr-2 px-4 py-2 bg-gray-500 text-white rounded-md"
                        >
                            Đóng
                        </button>
                        <button
                            type="submit"
                            :disabled="loading"
                            class="cursor-pointer hover:opacity-95 px-4 py-2 bg-blue-500 text-white rounded-md relative flex items-center justify-center"
                        >
                            <div v-if="loading" class="flex justify-center items-center absolute">
                                <div class="loader"></div>
                            </div>
                            <span :class="{ 'opacity-0': loading, 'transition-opacity duration-200': loading }"
                                >Thêm bài viết</span
                            >
                        </button>
                    </div>
                </form>
            </div>
        </div>

        <!-- View detail product -->
        <div
            v-if="selectedPost"
            class="fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center z-50 overflow-y-scroll"
        >
            <div @click.stop class="bg-white rounded-lg shadow-lg p-6 w-11/12 md:w-2/3 lg:w-1/2">
                <h2 class="text-xl font-bold text-center mb-4">Thông tin bài viết</h2>

                <div class="space-y-2">
                    <p><strong>Hình ảnh:</strong></p>
                    <div class="flex flex-wrap justify-start gap-3 mb-4">
                        <!-- <div v-for="(image, index) in selectedPost.images" :key="index" class="flex-shrink-0">
                            <img :src="image" alt="Hình ảnh bài viết" class="rounded-lg w-24 h-24 object-cover" />
                        </div> -->
                        <img
                            :src="selectedPost.image"
                            alt="Hình ảnh bài viết"
                            class="rounded-lg w-24 h-24 object-cover"
                        />
                    </div>
                    <p><strong>Bài viết: </strong> {{ selectedPost.text }}</p>
                    <p><strong>Người đăng: </strong> {{ selectedPost?.postedBy?.username }}</p>
                    <p><strong>Người xem:</strong> {{ selectedPost.numberViews }}</p>
                    <p><strong>Lượt thích:</strong> {{ selectedPost.likes.length }}</p>
                    <p><strong>Bình luận:</strong> {{ selectedPost.comments.length }}</p>

                    <p>
                        <strong>Chế độ:</strong>
                        <span
                            v-if="selectedPost.visibility === 'public'"
                            class="bg-blue-200 text-blue-600 py-1 px-3 rounded-full text-xs"
                        >
                            Công khai
                        </span>
                        <span
                            v-else-if="selectedPost.visibility === 'friends'"
                            class="bg-yellow-200 text-yellow-600 py-1 px-3 rounded-full text-xs"
                        >
                            Bạn bè
                        </span>
                        <span
                            v-else-if="selectedPost.visibility === 'followers'"
                            class="bg-green-200 text-green-600 py-1 px-3 rounded-full text-xs"
                        >
                            Người theo dõi
                        </span>
                        <span
                            v-else-if="selectedPost.visibility === 'private'"
                            class="bg-gray-200 text-gray-600 py-1 px-3 rounded-full text-xs"
                        >
                            Chỉ mình tôi
                        </span>
                    </p>
                    <p>
                        <strong>Chế độ:</strong>
                        <span
                            v-if="selectedPost.status === false"
                            class="bg-green-200 text-green-600 py-1 px-3 rounded-full text-xs"
                        >
                            Tích cực
                        </span>
                        <span
                            v-else-if="selectedPost.status === true"
                            class="bg-red-200 text-red-600 py-1 px-3 rounded-full text-xs"
                        >
                            Nhạy cảm
                        </span>
                    </p>

                    <p><strong>Ngày tạo:</strong> {{ formatDate(selectedPost.createdAt) }}</p>
                    <p><strong>Ngày cập nhật:</strong> {{ formatDate(selectedPost.updatedAt) }}</p>
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

        <!-- Edit product modal -->
        <div
            v-if="isEditPostModalVisible"
            class="fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center z-50"
        >
            <div class="bg-white rounded-lg p-6 w-11/12 md:w-2/3 lg:w-1/2 modal-content">
                <h2 class="text-lg font-bold mb-4">Chỉnh sửa thông tin bài viết</h2>
                <form @submit.prevent="saveEditedProduct">
                    <div class="mb-4">
                        <label for="editName" class="block text-sm font-medium text-gray-700">Tên</label>
                        <input
                            v-model="postToEdit.name"
                            type="text"
                            id="editName"
                            class="mt-1 p-2 w-full border border-gray-300 rounded-md"
                            required
                        />
                    </div>

                    <div class="mb-4">
                        <label for="editAuthor" class="block text-sm font-medium text-gray-700">Tác giả</label>
                        <input
                            v-model="postToEdit.author"
                            type="text"
                            id="editAuthor"
                            class="mt-1 p-2 w-full border border-gray-300 rounded-md"
                        />
                    </div>
                    <div class="mb-4">
                        <label for="editPrice" class="block text-sm font-medium text-gray-700">Gía</label>
                        <input
                            v-model="postToEdit.price"
                            type="number"
                            id="editPrice"
                            class="mt-1 p-2 w-full border border-gray-300 rounded-md"
                        />
                    </div>
                    <div class="mb-4">
                        <label for="editQuantity" class="block text-sm font-medium text-gray-700">Số lượng</label>
                        <input
                            v-model="postToEdit.quantity"
                            type="number"
                            id="editQuantity"
                            class="mt-1 p-2 w-full border border-gray-300 rounded-md"
                        />
                    </div>
                    <div class="mb-4">
                        <label for="editYearOfPublication" class="block text-sm font-medium text-gray-700"
                            >Năm XB</label
                        >
                        <input
                            v-model="postToEdit.yearOfPublication"
                            type="number"
                            id="editYearOfPublication"
                            class="mt-1 p-2 w-full border border-gray-300 rounded-md"
                        />
                    </div>
                    <div class="mb-4">
                        <label for="publisher" class="block text-sm font-medium text-gray-700">Nhà XB</label>
                        <!-- <select
                          v-model="postToEdit.publisherId.name"
                          id="publisher"
                          class="mt-1 p-2 w-full border border-gray-300 rounded-md"
                          required
                      >
                          <option disabled value="">Chọn nhà xuất bản</option>
                          <option v-for="publisher in publishers" :key="publisher._id" :value="publisher.name">
                              {{ publisher.name }}
                          </option>
                      </select> -->
                        <select
                            v-model="postToEdit.publisherId"
                            id="publisher"
                            class="mt-1 p-2 w-full border border-gray-300 rounded-md"
                        >
                            <option disabled value="">Chọn nhà xuất bản</option>
                            <option v-for="publisher in publishers" :key="publisher._id" :value="publisher._id">
                                {{ publisher.name }}
                            </option>
                        </select>
                    </div>

                    <div class="mb-4">
                        <label class="block text-sm font-medium text-gray-700">Ảnh hiện tại</label>
                        <div class="grid grid-cols-3 gap-4">
                            <div v-for="(image, index) in currentImages" :key="index" class="relative">
                                <img :src="image" alt="Current Image" class="w-full h-44 object-cover rounded-md" />
                                <button
                                    @click="removeCurrentImage(index)"
                                    type="button"
                                    class="absolute top-1 right-1 bg-red-500 text-white p-1 rounded"
                                >
                                    <i class="fas fa-trash-alt"></i>
                                </button>
                            </div>
                        </div>
                    </div>

                    <div class="mb-4">
                        <label class="block text-sm font-medium text-gray-700">Thêm ảnh mới</label>
                        <input
                            type="file"
                            id="newImages"
                            multiple
                            @change="handleNewFileChange"
                            class="mt-1 p-2 w-full border border-gray-300 rounded-md"
                        />
                    </div>

                    <div class="grid grid-cols-3 gap-4">
                        <div v-for="(image, index) in previewNewImages" :key="index" class="relative">
                            <img :src="image" alt="New Image Preview" class="w-full h-32 object-cover rounded-md" />
                        </div>
                    </div>

                    <div class="flex justify-end mt-6">
                        <button
                            type="button"
                            @click="closeEditProductModal"
                            class="cursor-pointer hover:opacity-95 mr-2 px-4 py-2 bg-gray-500 text-white rounded-md"
                        >
                            Đóng
                        </button>
                        <!-- <button
                          type="button"
                          @click="exportToPDF"
                          class="cursor-pointer hover:opacity-95 px-4 py-2 bg-green-500 text-white rounded-md"
                      >
                          Xuất PDF
                      </button> -->
                        <button
                            type="submit"
                            :disabled="loading"
                            class="cursor-pointer hover:opacity-95 px-4 py-2 bg-blue-500 text-white rounded-md relative flex items-center justify-center"
                        >
                            <div v-if="loading" class="flex justify-center items-center absolute">
                                <div class="loader"></div>
                            </div>
                            <span :class="{ 'opacity-0': loading, 'transition-opacity duration-200': loading }"
                                >Cập nhật</span
                            >
                        </button>
                    </div>
                </form>
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
            posts: [], // Dữ liệu bài viết
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
            newImages: [], // Danh bài viết file ảnh mới
            deletedImages: [], // Danh bài viết ảnh bị xóa
            publishers: [],
            isAddPostModalVisible: false,
            loading: false,
            loadingEdit: false,
            currentPage: 1, // Bắt đầu với trang đầu tiên
            pageSize: 10, // Hiển thị 10 bài viết mỗi trang
            selectedPost: null,
            postToEdit: null,
            isEditPostModalVisible: false, // For Edit product modal
            isLoading: false,
        };
    },
    computed: {
        totalPages() {
            return Math.ceil(this.filteredPosts.length / this.pageSize);
        },
        paginatedPosts() {
            const start = (this.currentPage - 1) * this.pageSize;
            return this.filteredPosts.slice(start, start + this.pageSize);
        },
        sortedAndPaginatedPosts() {
            // Sắp xếp dữ liệu
            const sortedPosts = this.sortedPosts;

            // Tính toán các chỉ số phân trang
            const start = (this.currentPage - 1) * this.pageSize;
            const end = start + this.pageSize;

            // Trả về dữ liệu phân trang sau khi đã sắp xếp
            return sortedPosts.slice(start, end);
        },
        sortedPosts() {
            // If no sort column is specified, return the Posts array as is
            const postsToSort = this.filteredPosts;
            if (!this.currentSort) {
                return postsToSort;
            }

            const direction = this.currentSortDir === 'asc' ? 1 : -1;
            return [...postsToSort].sort((a, b) => {
                let valueA = a[this.currentSort];
                let valueB = b[this.currentSort];

                if (this.currentSort === 'publisher') {
                    valueA = a.publisherId ? a.publisherId.name : '';
                    valueB = b.publisherId ? b.publisherId.name : '';
                }

                if (valueA < valueB) return -1 * direction;
                if (valueA > valueB) return 1 * direction;
                return 0;
            });
        },
        filteredPosts() {
            const searchQuery = this.searchQuery.toLowerCase().replace(/\//g, '');
            return this.posts.filter((post) => {
                const postedBy = post.postedBy?.username ? post.postedBy?.username.toLowerCase() : '';
                const text = post.text ? post.text.toLowerCase() : '';
                const visibility = post.visibility ? post.visibility.toLowerCase() : '';
                const publisher = post.publisherId?.name ? post.publisherId?.name.toLowerCase() : '';
                const createdAt = post.createdAt ? new Date(post.createdAt) : null;
                const updatedAt = post.updatedAt ? new Date(post.updatedAt) : null;

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
                    postedBy?.includes(searchQuery.toLowerCase()) ||
                    text.includes(searchQuery.toLowerCase()) ||
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

            // Chuyển đổi dữ liệu bài viết thành bảng
            const ws = XLSX.utils.json_to_sheet(this.posts);

            // Thêm bảng vào workbook
            XLSX.utils.book_append_sheet(wb, ws, 'Posts');

            // Tạo tên file
            const fileName = `posts_${new Date().toISOString().split('T')[0]}.xlsx`;

            // Xuất file
            XLSX.writeFile(wb, fileName);
        },
        exportToPDF() {
            // Tạo một tài liệu PDF mới
            const doc = new jsPDF();

            // Tiêu đề
            doc.setFontSize(20);
            doc.text('Danh bài viết sản phẩm', 10, 10);

            // Thêm tiêu đề bảng
            doc.setFontSize(12);
            const headers = ['Tên', 'Tác giả', 'Giá', 'Số lượng', 'Năm xuất bản', 'Nhà xuất bản'];
            const data = this.products.map((product) => [
                product.name,
                product.author,
                this.formatPrice(product.price),
                product.quantity,
                product.yearOfPublication,
                product.publisher,
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
        async fetchPosts() {
            this.isLoading = true;
            try {
                const user = JSON.parse(localStorage.getItem('user'));
                const userToken = user.accessToken;
                const res = await fetch('http://localhost:3001/api/post/getAllPostsFromAdmin', {
                    method: 'GET',
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `Bearer ${userToken}`,
                    },
                });
                const data = await res.json();
                const toast = useToast();
                console.log('dataPosts: ', data);

                if (!data.success) {
                    toast.error(data.message);
                    return;
                }
                this.posts = data.posts;
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
            this.resetNewPost(); // Đặt lại thông tin bài viết mới sau khi đóng modal
        },
        resetNewPost() {
            this.newPost = {
                name: '',
                author: '',
                price: null,
                quantity: null,
                yearOfPublication: null,
                publisher: '',
            };
            this.uploadImages = [];
            this.previewImages = [];
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
            const files = event.target.files; // Lấy danh bài viết file từ input
            if (files.length > 10) {
                // Nếu người dùng chọn quá 10 ảnh, có thể báo lỗi
                alert('Chọn tối đa 10 ảnh.');
                return;
            }

            this.uploadImages = []; // Xóa danh bài viết ảnh cũ
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

        async addPost() {
            this.loading = true;
            try {
                const user = JSON.parse(localStorage.getItem('user'));
                const userToken = user.accessToken;

                this.isUploading = true;

                const formData = new FormData();
                formData.append('postedBy', this.newPost.postedBy);
                formData.append('text', this.newPost.text);
                formData.append('visibility', this.newPost.visibility);
                // Thêm các ảnh đã chọn vào form data
                if (this.newPost.image) {
                    formData.append('image', this.newPost.image);
                }
                console.log('formData: ', formData);

                // Gửi request tạo sản phẩm cùng với ảnh
                const res = await fetch('http://localhost:3001/api/post/createPost', {
                    method: 'POST',
                    headers: {
                        Authorization: `Bearer ${userToken}`,
                    },
                    body: formData,
                });

                const data = await res.json();
                const toast = useToast();
                console.log('dataAddPosts: ', data);

                if (!data.success) {
                    toast.error(data.message);
                    return;
                }

                // Thêm sản phẩm mới vào danh bài viết
                this.posts.push(data.newPost);

                console.log('this.posts: ', this.posts);
                toast.success('Thêm bài viết thành công');
                this.closeAddPostModal();
            } catch (error) {
                console.error('Error adding product:', error);
                toast.error(error.message);
            } finally {
                this.loading = false;
            }
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

                // Cập nhật danh bài viết sản phẩm sau khi chỉnh sửa
                const index = this.products.findIndex((product) => product._id === productId);
                if (index !== -1) {
                    this.products[index] = data.updatedProduct;
                }

                toast.success('Cập nhật thông tin bài viết thành công');
                this.closeEditProductModal();
            } catch (error) {
                console.error('Error adding product:', error);
                const toast = useToast();
                toast.error(error.message);
            } finally {
                this.loading = false;
            }
        },

        viewPost(post) {
            // Logic xem chi tiết bài viết
            this.selectedPost = post;
            console.log('this.selectedPost: ', this.selectedPost);
        },
        closeModal() {
            this.selectedPost = null;
        },

        async deletePost(post) {
            if (!confirm('Bạn chắc chắn xoá bài viết này không?')) return;
            try {
                const userLocalStorage = JSON.parse(localStorage.getItem('user'));
                const userToken = userLocalStorage.accessToken;

                const res = await fetch(`http://localhost:3001/api/post/deletePostFromAdmin/${post._id}`, {
                    method: 'DELETE',
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `Bearer ${userToken}`,
                    },
                });

                const data = await res.json();
                const toast = useToast();
                console.log('dataDeletePost: ', data);

                if (!data.success) {
                    toast.error(data.message);
                    return;
                }
                this.posts.splice(
                    this.posts.findIndex((p) => p._id === post._id),
                    1,
                );
                toast.success('Xoá bài viết thành công');
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
        this.fetchPosts();
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
