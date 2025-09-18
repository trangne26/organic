<template>
  <div class="blog">
    <section class="page-header bg-green-50 py-12">
      <div class="container mx-auto px-4">
        <div class="text-center">
          <h1 class="text-3xl font-bold text-gray-800 mb-4">Blog</h1>
          <p class="text-gray-600">
            Chia sẻ kiến thức về ăn uống lành mạnh và lợi ích của thực phẩm hữu cơ
          </p>
        </div>
      </div>
    </section>

    <div class="container mx-auto px-4 py-8">
      <div class="grid grid-cols-1 lg:grid-cols-4 gap-8">
        <main class="lg:col-span-3">
          <div v-if="featuredPost" class="bg-white rounded-lg shadow-md overflow-hidden mb-8">
            <div class="relative">
              <img
                :src="featuredPost.image"
                :alt="featuredPost.title"
                class="w-full h-64 object-cover"
              />
              <div class="absolute top-4 left-4">
                <span class="bg-orange-500 text-white px-3 py-1 rounded-full text-sm font-semibold">
                  Nổi bật
                </span>
              </div>
            </div>
            <div class="p-6">
              <div class="flex items-center space-x-4 text-sm text-gray-600 mb-3">
                <span>📅 {{ formatDate(featuredPost.publishedAt) }}</span>
                <span>👤 {{ featuredPost.author }}</span>
                <span>🏷️ {{ featuredPost.category }}</span>
              </div>
              <router-link :to="`/blog/${featuredPost.slug}`">
                <h2 class="text-2xl font-bold text-gray-800 mb-3 hover:text-green-600 transition-colors">
                  {{ featuredPost.title }}
                </h2>
              </router-link>
              <p class="text-gray-600 mb-4">{{ featuredPost.excerpt }}</p>
              <router-link
                :to="`/blog/${featuredPost.slug}`"
                class="text-green-600 hover:text-green-700 font-semibold"
              >
                Đọc tiếp →
              </router-link>
            </div>
          </div>

          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
            <article
              v-for="post in posts"
              :key="post.id"
              class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition-shadow"
            >
              <div class="relative">
                <router-link :to="`/blog/${post.slug}`">
                  <img
                    :src="post.image"
                    :alt="post.title"
                    class="w-full h-48 object-cover"
                  />
                </router-link>
                <div class="absolute top-4 left-4">
                  <span class="bg-green-600 text-white px-2 py-1 rounded text-xs font-semibold">
                    {{ post.category }}
                  </span>
                </div>
              </div>
              <div class="p-4">
                <div class="flex items-center space-x-3 text-xs text-gray-600 mb-2">
                  <span>📅 {{ formatDate(post.publishedAt) }}</span>
                  <span>👤 {{ post.author }}</span>
                </div>
                <router-link :to="`/blog/${post.slug}`">
                  <h3 class="text-lg font-semibold text-gray-800 mb-2 hover:text-green-600 transition-colors">
                    {{ post.title }}
                  </h3>
                </router-link>
                <p class="text-gray-600 text-sm mb-3">{{ post.excerpt }}</p>
                <div class="flex items-center justify-between">
                  <router-link
                    :to="`/blog/${post.slug}`"
                    class="text-green-600 hover:text-green-700 font-semibold text-sm"
                  >
                    Đọc tiếp →
                  </router-link>
                  <span class="text-xs text-gray-500">{{ post.readTime }} phút đọc</span>
                </div>
              </div>
            </article>
          </div>

          <div v-if="totalPages > 1" class="flex justify-center">
            <nav class="flex space-x-2">
              <button
                v-for="page in totalPages"
                :key="page"
                :class="[
                  'px-4 py-2 rounded-md font-medium transition-colors',
                  page === currentPage
                    ? 'bg-green-600 text-white'
                    : 'bg-white text-gray-700 hover:bg-gray-100 border border-gray-300'
                ]"
                @click="currentPage = page"
              >
                {{ page }}
              </button>
            </nav>
          </div>
        </main>

        <aside class="lg:col-span-1">

          <div class="bg-white rounded-lg shadow-md p-6 mb-6">
            <h3 class="text-lg font-semibold text-gray-800 mb-4">Tìm kiếm</h3>
            <div class="relative">
              <input
                v-model="searchQuery"
                type="text"
                placeholder="Tìm kiếm bài viết..."
                class="w-full px-4 py-2 pr-10 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500 focus:border-transparent"
                @keyup.enter="search"
              />
              <button
                @click="search"
                class="absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-400 hover:text-green-600"
              >
                🔍
              </button>
            </div>
          </div>

          <div class="bg-white rounded-lg shadow-md p-6 mb-6">
            <h3 class="text-lg font-semibold text-gray-800 mb-4">Danh mục</h3>
            <ul class="space-y-2">
              <li
                v-for="category in categories"
                :key="category.slug"
                class="flex items-center justify-between"
              >
                <button
                  @click="filterByCategory(category.slug)"
                  :class="[
                    'text-sm transition-colors',
                    selectedCategory === category.slug
                      ? 'text-green-600 font-semibold'
                      : 'text-gray-600 hover:text-green-600'
                  ]"
                >
                  {{ category.name }}
                </button>
                <span class="text-xs text-gray-500 bg-gray-100 px-2 py-1 rounded-full">
                  {{ category.count }}
                </span>
              </li>
            </ul>
          </div>

          <div class="bg-white rounded-lg shadow-md p-6 mb-6">
            <h3 class="text-lg font-semibold text-gray-800 mb-4">Bài viết gần đây</h3>
            <div class="space-y-4">
              <div
                v-for="post in recentPosts"
                :key="post.id"
                class="flex space-x-3"
              >
                <router-link :to="`/blog/${post.slug}`" class="flex-shrink-0">
                  <img
                    :src="post.image"
                    :alt="post.title"
                    class="w-16 h-16 object-cover rounded-lg"
                  />
                </router-link>
                <div class="flex-1 min-w-0">
                  <router-link :to="`/blog/${post.slug}`">
                    <h4 class="text-sm font-semibold text-gray-800 hover:text-green-600 transition-colors line-clamp-2">
                      {{ post.title }}
                    </h4>
                  </router-link>
                  <p class="text-xs text-gray-600 mt-1">
                    {{ formatDate(post.publishedAt) }}
                  </p>
                </div>
              </div>
            </div>
          </div>

          <div class="bg-green-50 rounded-lg p-6">
            <h3 class="text-lg font-semibold text-gray-800 mb-4">Nhận tin tức mới nhất</h3>
            <p class="text-sm text-gray-600 mb-4">
              Đăng ký để nhận những bài viết hay và ưu đãi đặc biệt
            </p>
            <form @submit.prevent="subscribeNewsletter" class="space-y-3">
              <input
                v-model="newsletterEmail"
                type="email"
                placeholder="Email của bạn"
                required
                class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-green-500 focus:border-transparent"
              />
              <button
                type="submit"
                :disabled="subscribing"
                class="w-full bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded-md font-semibold transition-colors disabled:opacity-50"
              >
                {{ subscribing ? 'Đang đăng ký...' : 'Đăng ký' }}
              </button>
            </form>
          </div>
        </aside>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const searchQuery = ref('')
const selectedCategory = ref('')
const currentPage = ref(1)
const postsPerPage = 6
const newsletterEmail = ref('')
const subscribing = ref(false)

const featuredPost = ref({
  id: 1,
  title: '10 Lợi ích tuyệt vời của việc ăn thực phẩm hữu cơ',
  slug: '10-loi-ich-tuyet-voi-cua-viec-an-thuc-pham-huu-co',
  excerpt: 'Khám phá những lợi ích đáng kinh ngạc mà thực phẩm hữu cơ mang lại cho sức khỏe và môi trường...',
  content: '',
  image: '/images/blog/organic-benefits.jpg',
  author: 'Nguyễn Thị Lan',
  category: 'Sức khỏe',
  publishedAt: new Date('2024-01-15'),
  readTime: 5,
  isFeatured: true
})

const allPosts = ref([
  {
    id: 2,
    title: 'Cách bảo quản rau củ hữu cơ để giữ được lâu nhất',
    slug: 'cach-bao-quan-rau-cu-huu-co-de-giu-duoc-lau-nhat',
    excerpt: 'Học cách bảo quản rau củ hữu cơ đúng cách để duy trì độ tươi ngon và dinh dưỡng...',
    image: '/images/blog/vegetable-storage.jpg',
    author: 'Trần Văn Nam',
    category: 'Mẹo hay',
    publishedAt: new Date('2024-01-12'),
    readTime: 4
  },
  {
    id: 3,
    title: 'Công thức salad rau củ hữu cơ ngon miệng và bổ dưỡng',
    slug: 'cong-thuc-salad-rau-cu-huu-co-ngon-mieng-va-bo-duong',
    excerpt: 'Những công thức salad đơn giản nhưng vô cùng ngon miệng từ rau củ hữu cơ...',
    image: '/images/blog/organic-salad.jpg',
    author: 'Lê Thị Mai',
    category: 'Công thức',
    publishedAt: new Date('2024-01-10'),
    readTime: 6
  },
  {
    id: 4,
    title: 'Tại sao nên chọn thực phẩm hữu cơ cho trẻ em?',
    slug: 'tai-sao-nen-chon-thuc-pham-huu-co-cho-tre-em',
    excerpt: 'Tầm quan trọng của thực phẩm hữu cơ trong việc phát triển sức khỏe của trẻ...',
    image: '/images/blog/organic-for-kids.jpg',
    author: 'Phạm Thị Hoa',
    category: 'Sức khỏe',
    publishedAt: new Date('2024-01-08'),
    readTime: 5
  },
  {
    id: 5,
    title: 'Cách nhận biết thực phẩm hữu cơ thật và giả',
    slug: 'cach-nhan-biet-thuc-pham-huu-co-that-va-gia',
    excerpt: 'Những dấu hiệu giúp bạn phân biệt thực phẩm hữu cơ thật và giả một cách chính xác...',
    image: '/images/blog/identify-organic.jpg',
    author: 'Hoàng Văn Đức',
    category: 'Kiến thức',
    publishedAt: new Date('2024-01-05'),
    readTime: 7
  },
  {
    id: 6,
    title: 'Xu hướng ăn uống lành mạnh năm 2024',
    slug: 'xu-huong-an-uong-lanh-manh-nam-2024',
    excerpt: 'Cập nhật những xu hướng ăn uống lành mạnh mới nhất trong năm 2024...',
    image: '/images/blog/healthy-trends-2024.jpg',
    author: 'Ngô Thị Linh',
    category: 'Xu hướng',
    publishedAt: new Date('2024-01-03'),
    readTime: 8
  }
])

const categories = ref([
  { name: 'Tất cả', slug: '', count: 6 },
  { name: 'Sức khỏe', slug: 'suc-khoe', count: 2 },
  { name: 'Công thức', slug: 'cong-thuc', count: 1 },
  { name: 'Mẹo hay', slug: 'meo-hay', count: 1 },
  { name: 'Kiến thức', slug: 'kien-thuc', count: 1 },
  { name: 'Xu hướng', slug: 'xu-huong', count: 1 }
])

const posts = computed(() => {
  let filtered = allPosts.value

  if (selectedCategory.value) {
    filtered = filtered.filter(post => post.category.toLowerCase().replace(/\s+/g, '-') === selectedCategory.value)
  }

  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase()
    filtered = filtered.filter(post =>
      post.title.toLowerCase().includes(query) ||
      post.excerpt.toLowerCase().includes(query)
    )
  }

  const start = (currentPage.value - 1) * postsPerPage
  const end = start + postsPerPage
  return filtered.slice(start, end)
})

const totalPages = computed(() => {
  let filtered = allPosts.value

  if (selectedCategory.value) {
    filtered = filtered.filter(post => post.category.toLowerCase().replace(/\s+/g, '-') === selectedCategory.value)
  }

  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase()
    filtered = filtered.filter(post =>
      post.title.toLowerCase().includes(query) ||
      post.excerpt.toLowerCase().includes(query)
    )
  }

  return Math.ceil(filtered.length / postsPerPage)
})

const recentPosts = computed(() => {
  return [...allPosts.value]
    .sort((a, b) => new Date(b.publishedAt) - new Date(a.publishedAt))
    .slice(0, 4)
})

const formatDate = (date) => {
  return new Intl.DateTimeFormat('vi-VN', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  }).format(date)
}

const search = () => {
  currentPage.value = 1
}

const filterByCategory = (categorySlug) => {
  selectedCategory.value = categorySlug
  currentPage.value = 1
}

const subscribeNewsletter = async () => {
  subscribing.value = true

  setTimeout(() => {
    alert('Đăng ký thành công! Cảm ơn bạn đã quan tâm.')
    newsletterEmail.value = ''
    subscribing.value = false
  }, 1500)
}

onMounted(() => {
})
</script>

<style scoped>
.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
</style>
