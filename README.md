# 🌱 Organic Store - Website Bán Thực Phẩm Hữu Cơ

Một website bán thực phẩm hữu cơ hiện đại được xây dựng với Vue 3 + Vite + Tailwind CSS.

## ✨ Tính năng

### 🏠 Trang chủ
- Hero banner với sản phẩm nổi bật
- Danh mục sản phẩm với icons
- Sản phẩm mới nhất và bán chạy
- Cam kết chất lượng

### 🛍️ Sản phẩm
- **Danh sách sản phẩm** với bộ lọc và tìm kiếm
- **Chi tiết sản phẩm** với ảnh, mô tả, đánh giá
- **Giỏ hàng** với quản lý số lượng và mã giảm giá
- **Thanh toán** 3 bước: thông tin → giao hàng → thanh toán

### 👤 Tài khoản
- Đăng ký/Đăng nhập với validation
- Trang cá nhân với lịch sử đơn hàng
- Wishlist sản phẩm yêu thích

### 📖 Nội dung
- **Blog** với bài viết về thực phẩm hữu cơ
- **Giới thiệu** công ty và đội ngũ
- **Liên hệ** với form và thông tin cửa hàng

## 🛠️ Công nghệ sử dụng

- **Vue 3** - Framework JavaScript
- **Vite** - Build tool
- **Vue Router 4** - Routing
- **Tailwind CSS** - Styling
- **Composition API** - Vue 3 features

## 📦 Cài đặt

### Yêu cầu
- Node.js >= 16.0.0
- npm hoặc yarn

### Bước 1: Clone dự án
```bash
git clone <repository-url>
cd organic
```

### Bước 2: Cài đặt dependencies
```bash
npm install
```

### Bước 3: Chạy development server
```bash
npm run dev
```

Ứng dụng sẽ chạy tại: `http://localhost:5173`

## 📝 Scripts

```bash
# Chạy development server
npm run dev

# Build cho production
npm run build

# Preview build
npm run preview

# Linting
npm run lint
```

## 🎨 Thiết kế

### Màu sắc chính
- **Primary (Green)**: `#16a34a` - Cho CTA và elements chính
- **Secondary (Orange)**: `#f97316` - Cho giá và khuyến mãi
- **Neutral**: Các tone xám cho text và background

### Typography
- Font chính: **Inter** (fallback: system fonts)
- Responsive typography với Tailwind utilities

### Layout
- **Mobile-first** responsive design
- **Container** max-width: 1280px
- **Grid system** với Tailwind CSS

## 📁 Cấu trúc dự án

```
src/
├── assets/          # CSS, images, icons
├── components/      # Vue components
│   └── layout/      # Layout components (Header, Footer)
├── router/          # Vue Router configuration
├── views/           # Page components
│   ├── auth/        # Authentication pages
│   ├── content/     # Content pages (Blog, About, Contact)
│   └── products/    # Product pages
└── main.js          # App entry point
```

## 🚀 Deployment

### Build cho production
```bash
npm run build
```

Thư mục `dist/` sẽ chứa các file để deploy.

### Deploy lên Netlify/Vercel
1. Build project: `npm run build`
2. Upload thư mục `dist/` 
3. Cấu hình SPA routing (redirect tất cả về `index.html`)

## 🔧 Cấu hình

### Tailwind CSS
File cấu hình: `tailwind.config.js`
- Custom colors cho brand
- Extended utilities
- Responsive breakpoints

### Vue Router
File cấu hình: `src/router/index.js`
- All routes defined
- Navigation guards
- Meta tags cho SEO

### Vite
File cấu hình: `vite.config.js`
- Path aliases (`@` = `src/`)
- Dev tools enabled

## 📱 Responsive Design

- **Mobile**: < 768px
- **Tablet**: 768px - 1024px  
- **Desktop**: > 1024px

Tất cả components được thiết kế responsive với Tailwind breakpoints.

## 🎯 Features chính

### ✅ Đã hoàn thành
- [x] Router và navigation
- [x] Tất cả pages/views
- [x] Layout components (Header, Footer)
- [x] Responsive design
- [x] Tailwind CSS setup
- [x] Mock data cho development

### 🔄 Có thể mở rộng
- [ ] State management (Pinia/Vuex)
- [ ] API integration
- [ ] Authentication backend
- [ ] Payment gateway
- [ ] Image optimization
- [ ] SEO meta tags
- [ ] PWA features

## 🐛 Debugging

### Common issues
1. **Tailwind classes không hoạt động**: Kiểm tra `tailwind.config.js` content paths
2. **Router không hoạt động**: Đảm bảo Vue Router được import đúng trong `main.js`
3. **Components không render**: Kiểm tra import paths và component names

### Development tools
- Vue DevTools browser extension
- Vite HMR (Hot Module Replacement)
- Browser developer tools

## 📄 License

MIT License - có thể sử dụng tự do cho mục đích học tập và thương mại.

## 👥 Contributing

1. Fork the project
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

## 📞 Hỗ trợ

Nếu gặp vấn đề, hãy tạo issue trên GitHub hoặc liên hệ qua email.

---

**Lưu ý**: Đây là dự án MVP phục vụ mục đích học tập. Dữ liệu hiện tại là mock data, cần tích hợp với backend thực tế để sử dụng trong production.