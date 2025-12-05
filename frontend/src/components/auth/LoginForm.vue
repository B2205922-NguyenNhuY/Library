<template>
  <div class="login-background">
    <div class="login-box">
      <!-- Hình ảnh. -->
      <div class="login-left">
        <div class="image-wrapper">
          <img src="../../assets/Library-amico.png" class="login-image" />
        </div>
      </div>

      <!-- Form đăng nhập -->
      <div class="login-right">
        <LoadingSpinner :show="loading" />

        <div class="login-title">
          <h1>Đăng nhập</h1>
          <p>Đăng nhập để truy cập tài khoản của bạn</p>
        </div>

        <!-- Tab chọn loại người dùng -->
        <div class="user-type-tabs">
          <button
            class="tab-button"
            :class="{ active: userType === 'reader' }"
            @click="userType = 'reader'"
          >
            Độc giả
          </button>
          <button
            class="tab-button"
            :class="{ active: userType === 'staff' }"
            @click="userType = 'staff'"
          >
            Nhân viên
          </button>
        </div>

        <form @submit.prevent="handleSubmit" novalidate class="login-form">
          <!-- Thông báo lỗi -->
          <div v-if="error" class="alert alert-error">
            {{ error }}
            <button type="button" class="alert-close" @click="clearError">
              ×
            </button>
          </div>

          <!-- MSNV cho nhân viên -->
          <div class="form-group" v-if="userType === 'staff'">
            <label>MSNV <span class="required">*</span></label>
            <input
              v-model="credentials.MSNV"
              type="text"
              class="form-control"
              :class="{ 'is-invalid': errors.MSNV }"
              placeholder="Nhập MSNV"
              required
              @input="validateField('MSNV')"
            />
            <div class="error-message" v-if="errors.MSNV">
              {{ errors.MSNV }}
            </div>
          </div>

          <!-- Email cho độc giả -->
          <div class="form-group" v-if="userType === 'reader'">
            <label>Email <span class="required">*</span></label>
            <input
              v-model="credentials.email"
              type="email"
              class="form-control"
              :class="{ 'is-invalid': errors.email }"
              placeholder="Nhập email"
              required
              @input="validateField('email')"
            />
            <div class="error-message" v-if="errors.email">
              {{ errors.email }}
            </div>
          </div>

          <!-- Mật khẩu -->
          <div class="form-group password-group">
            <label>Mật khẩu <span class="required">*</span></label>
            <div class="password-input-wrapper">
              <input
                v-model="credentials.password"
                :type="showPassword ? 'text' : 'password'"
                class="form-control"
                :class="{ 'is-invalid': errors.password }"
                placeholder="Nhập mật khẩu"
                required
                @input="validateField('password')"
              />
              <button
                type="button"
                class="password-toggle"
                @click="togglePasswordVisibility"
              >
                <i
                  :class="showPassword ? 'fas fa-eye-slash' : 'fas fa-eye'"
                ></i>
              </button>
            </div>
            <div class="error-message" v-if="errors.password">
              {{ errors.password }}
            </div>
          </div>

          <button type="submit" class="btn-login" :disabled="loading">
            {{ loading ? "Đang xử lý..." : "Đăng nhập" }}
          </button>

          <p class="register-link">
            Chưa có tài khoản?
            <router-link to="/register" class="text-decoration-none fw-bold">
              Đăng ký ngay
            </router-link>
          </p>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { useStore } from "vuex";
import { useRouter } from "vue-router";
import LoadingSpinner from "@/components/LoadingSpinner.vue";
import { showError } from "@/utils/notifications";

export default {
  name: "LoginForm",
  components: { LoadingSpinner },
  setup() {
    const store = useStore();
    const router = useRouter();
    const userType = ref("reader");
    const credentials = ref({
      MSNV: "",
      email: "",
      password: "",
    });
    const errors = ref({});
    const loginAttempts = ref(0);
    const showPassword = ref(false);

    const loading = computed(() => store.state.auth.loading);
    const error = computed(() => store.state.auth.error);

    const validateField = (field) => {
      const newErrors = { ...errors.value };

      if (field === "MSNV" && userType.value === "staff") {
        if (!credentials.value.MSNV) {
          newErrors.MSNV = "MSNV là bắt buộc";
        } else {
          delete newErrors.MSNV;
        }
      }

      if (field === "email" && userType.value === "reader") {
        if (!credentials.value.email) {
          newErrors.email = "Email là bắt buộc";
        } else if (
          !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(credentials.value.email)
        ) {
          newErrors.email = "Email không hợp lệ";
        } else {
          delete newErrors.email;
        }
      }

      if (field === "password") {
        if (!credentials.value.password) {
          newErrors.password = "Mật khẩu là bắt buộc";
        } else if (credentials.value.password.length < 5) {
          newErrors.password = "Mật khẩu phải có ít nhất 5 ký tự";
        } else if (
          !/(?=.*[A-Za-z])(?=.*\d).+/.test(credentials.value.password)
        ) {
          newErrors.password = "Mật khẩu phải chứa cả chữ và số";
        } else {
          delete newErrors.password;
        }
      }

      errors.value = newErrors;
    };

    const validateForm = () => {
      const newErrors = {};
      if (userType.value === "staff" && !credentials.value.MSNV) {
        newErrors.MSNV = "MSNV là bắt buộc";
      }
      if (userType.value === "reader" && !credentials.value.email) {
        newErrors.email = "Email là bắt buộc";
      } else if (
        userType.value === "reader" &&
        !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(credentials.value.email)
      ) {
        newErrors.email = "Email không hợp lệ";
      }
      if (!credentials.value.password) {
        newErrors.password = "Mật khẩu là bắt buộc";
      } else if (credentials.value.password.length < 5) {
        newErrors.password = "Mật khẩu phải có ít nhất 5 ký tự";
      } else if (!/(?=.*[A-Za-z])(?=.*\d).+/.test(credentials.value.password)) {
        newErrors.password = "Mật khẩu phải chứa cả chữ và số";
      }
      errors.value = newErrors;
      return Object.keys(newErrors).length === 0;
    };

    const handleSubmit = async () => {
      if (!validateForm()) {
        return;
      }

      try {
        loginAttempts.value += 1;
        if (userType.value === "staff") {
          await store.dispatch("auth/loginStaff", {
            MSNV: credentials.value.MSNV,
            password: credentials.value.password,
          });
          router.push("/admin");
        } else {
          await store.dispatch("auth/loginReader", {
            email: credentials.value.email,
            password: credentials.value.password,
          });
          router.push("/reader");
        }
        loginAttempts.value = 0;
        errors.value = {};
      } catch (err) {
        showError(err);
      }
    };

    const clearError = () => {
      store.commit("auth/SET_ERROR", null);
    };

    const togglePasswordVisibility = () => {
      showPassword.value = !showPassword.value;
    };

    return {
      userType,
      credentials,
      loading,
      error,
      errors,
      loginAttempts,
      showPassword,
      togglePasswordVisibility,
      handleSubmit,
      clearError,
      validateField,
    };
  },
};
</script>

<style scoped>
* {
  box-sizing: border-box;
}

.login-background {
  min-height: 100vh;
  background: linear-gradient(135deg, #E8F5E9 0%, #A5D6A7 100%);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

.login-box {
  display: flex;
  width: 100%;
  max-width: 1200px;
  height: 650px;
  background: white;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(67, 160, 71, 0.15);
  overflow: hidden;
}

.login-left {
  flex: 1;
  background: linear-gradient(135deg, #A5D6A7 0%, #66BB6A 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 30px;
}

.image-wrapper {
  max-width: 80%;
}

.login-image {
  width: 100%;
  height: auto;
  object-fit: contain;
}

.login-right {
  flex: 1;
  padding: 25px 35px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  background: white;
  position: relative;
}

.login-title {
  text-align: center;
  margin-bottom: 20px;
}

.login-title h1 {
  font-size: 1.8rem;
  font-weight: 600;
  color: #2E7D32;
  margin-bottom: 8px;
}

.login-title p {
  font-size: 0.95rem;
  color: #78909c;
}

.user-type-tabs {
  display: flex;
  margin-bottom: 20px;
  border-radius: 8px;
  overflow: hidden;
  border: 1px solid #C8E6C9;
}

.tab-button {
  flex: 1;
  padding: 10px 16px;
  border: none;
  background: #F1F8E9;
  color: #2E7D32;
  font-size: 0.9rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
}

.tab-button.active {
  background: #43A047;
  color: white;
}

.tab-button:hover:not(.active) {
  background: #E8F5E9;
}

.login-form {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.form-group label {
  font-size: 0.85rem;
  font-weight: 500;
  color: #2E7D32;
  margin-bottom: 4px;
}

.required {
  color: #d32f2f;
}

.form-control {
  padding: 10px 12px;
  border: 1px solid #C8E6C9;
  border-radius: 8px;
  font-size: 0.9rem;
  transition: border-color 0.2s ease;
  background: #f9fef9;
  color: #2E7D32;
}

.form-control:focus {
  outline: none;
  border-color: #43A047;
  background: white;
  box-shadow: 0 0 0 2px rgba(76, 175, 80, 0.15);
}

.password-toggle:hover {
  color: #43A047;
}

.btn-login {
  background: linear-gradient(135deg, #43A047 0%, #2E7D32 100%);
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 8px;
  font-size: 0.95rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
  margin-top: 8px;
  box-shadow: 0 2px 8px rgba(76, 175, 80, 0.3);
  width: 100%;
}

.btn-login:hover:not(:disabled) {
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(76, 175, 80, 0.45);
}

.alert-error {
  background: #FFEBEE;
  color: #C62828;
  border-left: 4px solid #E57373;
}

.register-link a {
  color: #43A047;
  font-weight: 600;
  text-decoration: none;
}

.register-link a:hover {
  color: #2E7D32;
}
</style>
