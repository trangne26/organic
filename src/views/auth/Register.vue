<template>
  <div class="register">
    <div class="min-h-screen flex items-center justify-center bg-gray-50 py-12 px-4 sm:px-6 lg:px-8">
      <div class="max-w-md w-full space-y-8">
        <div>
          <div class="mx-auto h-12 w-12 text-center">
            <span class="text-4xl">🌱</span>
          </div>
          <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">
            Tạo tài khoản mới
          </h2>
          <p class="mt-2 text-center text-sm text-gray-600">
            Hoặc
            <router-link
              to="/login"
              class="font-medium text-green-600 hover:text-green-500"
            >
              đăng nhập với tài khoản có sẵn
            </router-link>
          </p>
        </div>
        
        <form class="mt-8 space-y-6" @submit.prevent="handleRegister">
          <div class="space-y-4">
            <div>
              <label for="fullName" class="block text-sm font-medium text-gray-700">
                Họ và tên *
              </label>
              <input
                id="fullName"
                v-model="form.fullName"
                name="fullName"
                type="text"
                autocomplete="name"
                required
                class="mt-1 appearance-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-md focus:outline-none focus:ring-green-500 focus:border-green-500 focus:z-10 sm:text-sm"
                placeholder="Nhập họ và tên"
              />
            </div>

            <div>
              <label for="email" class="block text-sm font-medium text-gray-700">
                Email *
              </label>
              <input
                id="email"
                v-model="form.email"
                name="email"
                type="email"
                autocomplete="email"
                required
                class="mt-1 appearance-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-md focus:outline-none focus:ring-green-500 focus:border-green-500 focus:z-10 sm:text-sm"
                placeholder="Nhập địa chỉ email"
              />
            </div>

            <div>
              <label for="phone" class="block text-sm font-medium text-gray-700">
                Số điện thoại *
              </label>
              <input
                id="phone"
                v-model="form.phone"
                name="phone"
                type="tel"
                autocomplete="tel"
                required
                class="mt-1 appearance-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-md focus:outline-none focus:ring-green-500 focus:border-green-500 focus:z-10 sm:text-sm"
                placeholder="Nhập số điện thoại"
              />
            </div>

            <div>
              <label for="password" class="block text-sm font-medium text-gray-700">
                Mật khẩu *
              </label>
              <input
                id="password"
                v-model="form.password"
                name="password"
                type="password"
                autocomplete="new-password"
                required
                class="mt-1 appearance-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-md focus:outline-none focus:ring-green-500 focus:border-green-500 focus:z-10 sm:text-sm"
                placeholder="Nhập mật khẩu (tối thiểu 6 ký tự)"
                minlength="6"
              />
            </div>

            <div>
              <label for="confirmPassword" class="block text-sm font-medium text-gray-700">
                Xác nhận mật khẩu *
              </label>
              <input
                id="confirmPassword"
                v-model="form.confirmPassword"
                name="confirmPassword"
                type="password"
                autocomplete="new-password"
                required
                class="mt-1 appearance-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-md focus:outline-none focus:ring-green-500 focus:border-green-500 focus:z-10 sm:text-sm"
                placeholder="Nhập lại mật khẩu"
              />
            </div>
          </div>

          <!-- Password Requirements -->
          <div class="bg-blue-50 border border-blue-200 rounded-md p-3">
            <h4 class="text-sm font-medium text-blue-800 mb-2">Yêu cầu mật khẩu:</h4>
            <ul class="text-xs text-blue-700 space-y-1">
              <li :class="form.password.length >= 6 ? 'text-green-600' : ''">
                ✓ Tối thiểu 6 ký tự
              </li>
              <li :class="hasUpperCase ? 'text-green-600' : ''">
                ✓ Có ít nhất 1 chữ hoa
              </li>
              <li :class="hasNumber ? 'text-green-600' : ''">
                ✓ Có ít nhất 1 số
              </li>
            </ul>
          </div>

          <div class="flex items-center">
            <input
              id="agree-terms"
              v-model="form.agreeTerms"
              name="agree-terms"
              type="checkbox"
              required
              class="h-4 w-4 text-green-600 focus:ring-green-500 border-gray-300 rounded"
            />
            <label for="agree-terms" class="ml-2 block text-sm text-gray-900">
              Tôi đồng ý với
              <a href="#" class="text-green-600 hover:text-green-500 font-medium">
                Điều khoản dịch vụ
              </a>
              và
              <a href="#" class="text-green-600 hover:text-green-500 font-medium">
                Chính sách bảo mật
              </a>
            </label>
          </div>

          <!-- Error Message -->
          <div v-if="error" class="bg-red-50 border border-red-200 text-red-600 px-4 py-3 rounded">
            {{ error }}
          </div>

          <!-- Success Message -->
          <div v-if="success" class="bg-green-50 border border-green-200 text-green-600 px-4 py-3 rounded">
            {{ success }}
          </div>

          <div>
            <button
              type="submit"
              :disabled="loading || !isFormValid"
              class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-green-600 hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500 disabled:opacity-50 disabled:cursor-not-allowed"
            >
              <span class="absolute left-0 inset-y-0 flex items-center pl-3">
                <svg
                  class="h-5 w-5 text-green-500 group-hover:text-green-400"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                  fill="currentColor"
                  aria-hidden="true"
                >
                  <path
                    fill-rule="evenodd"
                    d="M10 9a3 3 0 100-6 3 3 0 000 6zm-7 9a7 7 0 1114 0H3z"
                    clip-rule="evenodd"
                  />
                </svg>
              </span>
              {{ loading ? 'Đang tạo tài khoản...' : 'Tạo tài khoản' }}
            </button>
          </div>

          <!-- Social Register -->
          <div class="mt-6">
            <div class="relative">
              <div class="absolute inset-0 flex items-center">
                <div class="w-full border-t border-gray-300" />
              </div>
              <div class="relative flex justify-center text-sm">
                <span class="px-2 bg-gray-50 text-gray-500">Hoặc đăng ký với</span>
              </div>
            </div>

            <div class="mt-6 grid grid-cols-2 gap-3">
              <button
                type="button"
                class="w-full inline-flex justify-center py-2 px-4 border border-gray-300 rounded-md shadow-sm bg-white text-sm font-medium text-gray-500 hover:bg-gray-50"
              >
                <span class="sr-only">Đăng ký với Google</span>
                <svg class="w-5 h-5" viewBox="0 0 24 24">
                  <path
                    fill="currentColor"
                    d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"
                  />
                  <path
                    fill="currentColor"
                    d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"
                  />
                  <path
                    fill="currentColor"
                    d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z"
                  />
                  <path
                    fill="currentColor"
                    d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"
                  />
                </svg>
                <span class="ml-2">Google</span>
              </button>

              <button
                type="button"
                class="w-full inline-flex justify-center py-2 px-4 border border-gray-300 rounded-md shadow-sm bg-white text-sm font-medium text-gray-500 hover:bg-gray-50"
              >
                <span class="sr-only">Đăng ký với Facebook</span>
                <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
                </svg>
                <span class="ml-2">Facebook</span>
              </button>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const form = ref({
  fullName: '',
  email: '',
  phone: '',
  password: '',
  confirmPassword: '',
  agreeTerms: false
})

const loading = ref(false)
const error = ref('')
const success = ref('')

const hasUpperCase = computed(() => {
  return /[A-Z]/.test(form.value.password)
})

const hasNumber = computed(() => {
  return /\d/.test(form.value.password)
})

const isFormValid = computed(() => {
  return (
    form.value.fullName.trim() &&
    form.value.email.trim() &&
    form.value.phone.trim() &&
    form.value.password.length >= 6 &&
    form.value.password === form.value.confirmPassword &&
    form.value.agreeTerms
  )
})

const handleRegister = async () => {
  loading.value = true
  error.value = ''
  success.value = ''
  
  // Validate passwords match
  if (form.value.password !== form.value.confirmPassword) {
    error.value = 'Mật khẩu xác nhận không khớp'
    loading.value = false
    return
  }
  
  try {
    // Simulate API call
    await new Promise((resolve, reject) => {
      setTimeout(() => {
        // Mock registration - check if email already exists
        if (form.value.email === 'existing@example.com') {
          reject(new Error('Email này đã được sử dụng'))
        } else {
          resolve()
        }
      }, 2000)
    })
    
    success.value = 'Tài khoản đã được tạo thành công! Đang chuyển hướng...'
    
    // Redirect to login after success
    setTimeout(() => {
      router.push('/login')
    }, 2000)
    
  } catch (err) {
    error.value = err.message
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
/* Custom styles for better visual appearance */
.register {
  background-image: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1000 1000"><defs><radialGradient id="a" cx="50%" cy="50%"><stop offset="0%" stop-color="%23f0fdf4"/><stop offset="100%" stop-color="%23dcfce7"/></radialGradient></defs><rect width="100%" height="100%" fill="url(%23a)"/></svg>');
}
</style>
