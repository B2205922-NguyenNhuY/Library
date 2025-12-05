<template>
  <div class="home-background">
    <div class="home-container">
      <!-- ,Left side - Content -->
      <div class="home-left">
        <div class="home-content">
          <div class="home-title">
            <h1>Hệ thống quản lý thư viện</h1>
            <p class="home-subtitle">
              Chào mừng bạn đến với thư viện trực tuyến
            </p>
          </div>
          <div class="home-description">
            <p>
              Khám phá kho tàng tri thức phong phú với hàng ngàn đầu sách đa
              dạng. Đăng nhập tài khoản để dễ dàng tìm kiếm, mượn và quản lý
              sách yêu thích của bạn.
            </p>
          </div>
          <div class="home-actions">
            <router-link to="/login" class="btn-primary">
              <i class="fas fa-user"></i>
              Đăng nhập
            </router-link>
            <router-link to="/register" class="btn-secondary">
              <i class="fas fa-user-plus"></i>
              Đăng ký
            </router-link>
          </div>
        </div>
      </div>
      <!-- Right side - Image/Illustration -->
      <div class="home-right">
        <div class="image-wrapper">
          <img src="../assets/Bibliophile-rafiki.png" class="home-image" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HomeView",
};
</script>

<style scoped>
* {
  box-sizing: border-box;
}

/* ==== BACKGROUND ==== */
.home-background {
  min-height: 100vh;
  background: linear-gradient(135deg, #E8F5E9 0%, #C8E6C9 50%, #A5D6A7 100%);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 20px;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  position: relative;
  overflow: hidden;
}

/* ==== WRAPPER ==== */
.home-container {
  display: flex;
  width: 100%;
  max-width: 1200px;
  min-height: 600px;
  background: white;
  border-radius: 20px;
  box-shadow: 0 20px 60px rgba(76, 175, 80, 0.15);
  overflow: hidden;
  position: relative;
  z-index: 2;
}

/* ==== LEFT CONTENT ==== */
.home-left {
  flex: 1;
  padding: 50px 40px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background: linear-gradient(135deg, #ffffff 0%, #F1F8E9 100%);
}

.home-content {
  max-width: 500px;
}

/* ==== TITLE ==== */
.home-title {
  margin-bottom: 30px;
}

.home-title h1 {
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 15px;
  line-height: 1.2;
  background: linear-gradient(135deg, #1B5E20 0%, #43A047 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.home-subtitle {
  font-size: 1.2rem;
  color: #33691E;
  font-weight: 400;
  margin: 0;
}

/* ==== DESCRIPTION ==== */
.home-description {
  margin-bottom: 35px;
}
.home-description p {
  font-size: 1rem;
  color: #4E6E4E;
  line-height: 1.6;
  margin: 0;
}

/* ==== BUTTONS ==== */
.home-actions {
  display: flex;
  gap: 15px;
  flex-wrap: wrap;
}

.btn-primary,
.btn-secondary {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 14px 32px;
  border-radius: 12px;
  font-size: 0.95rem;
  font-weight: 600;
  text-decoration: none;
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
}

/* Primary button GREEN */
.btn-primary {
  background: linear-gradient(135deg, #43A047 0%, #2E7D32 100%);
  color: white;
}

.btn-primary:hover {
  transform: translateY(-2px);
  color: white;
  background: linear-gradient(135deg, #2E7D32 0%, #1B5E20 100%);
}

/* Secondary outline GREEN */
.btn-secondary {
  background: transparent;
  color: #43A047;
  border: 2px solid #43A047;
}

.btn-secondary:hover {
  background: #43A047;
  color: white;
  transform: translateY(-2px);
}

/* ==== RIGHT IMAGE ==== */
.home-right {
  flex: 1;
  background: linear-gradient(135deg, #A5D6A7 0%, #66BB6A 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 40px;
  position: relative;
  overflow: hidden;
}

.image-wrapper {
  max-width: 85%;
  z-index: 2;
  position: relative;
}

.home-image {
  width: 100%;
  height: auto;
  object-fit: contain;
  filter: drop-shadow(0 10px 30px rgba(0, 0, 0, 0.1));
}
</style>

