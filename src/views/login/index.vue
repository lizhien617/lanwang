<template>
  <div class="login-page">
    <!-- 左侧登录表单 -->
    <div class="login-form-container">
      <h2 class="welcome-title">Welcome back</h2>
      <p class="subtitle">Welcome back! Please enter your details.</p>

      <el-form ref="loginForm" :model="loginForm" :rules="loginRules" label-position="top" class="form-box">
        <el-form-item label="Username" prop="username">
          <el-input v-model="loginForm.username" placeholder="Enter your username" />
        </el-form-item>

        <el-form-item label="Password" prop="password">
          <el-input
            :key="passwordType"
            ref="password"
            v-model="loginForm.password"
            :type="passwordType"
            placeholder="Enter your password"
            name="password"
            tabindex="2"
            auto-complete="on"
            @keyup.enter.native="handleLogin"
          >
            <template slot="suffix">
              <svg-icon
                :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'"
                style="cursor: pointer; font-size: 18px; color: #999;"
                @click.native="showPwd"
              />
            </template>
          </el-input>
        </el-form-item>

        <el-button type="primary" class="signin-btn" @click="handleLogin">Sign in</el-button>

        <p class="signup-tip">
          Don’t have an account?
          <el-link type="primary" :underline="true">Sign up for free</el-link>
        </p>
      </el-form>
    </div>

    <!-- 右侧图文区 -->
    <div class="login-side-image">
      <div class="overlay-card">
        <p class="quote">“We’ve been using Untitled to kick start every new project and can’t imagine working without it.”</p>
        <p class="author">Andi Lane</p>
        <p class="meta">Founder, Catalog<br>Web Design Agency</p>
      </div>
    </div>
  </div>
</template>

<script>
import { validUsername } from '@/utils/validate'

export default {
  name: 'Login',
  data() {
    const validateUsername = (rule, value, callback) => {
      if (!validUsername(value)) {
        callback(new Error('Please enter the correct user name'))
      } else {
        callback()
      }
    }
    const validatePassword = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error('The password can not be less than 6 digits'))
      } else {
        callback()
      }
    }
    return {
      loginForm: {
        username: 'admin',
        password: '111111'
      },
      loginRules: {
        username: [{ required: true, trigger: 'blur', validator: validateUsername }],
        password: [{ required: true, trigger: 'blur', validator: validatePassword }]
      },
      loading: false,
      passwordType: 'password',
      redirect: undefined
    }
  },
  watch: {
    $route: {
      handler: function(route) {
        this.redirect = route.query && route.query.redirect
      },
      immediate: true
    }
  },
  methods: {
    showPwd() {
      if (this.passwordType === 'password') {
        this.passwordType = ''
      } else {
        this.passwordType = 'password'
      }
      this.$nextTick(() => {
        this.$refs.password.focus()
      })
    },
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true
          this.$store.dispatch('user/login', this.loginForm).then(() => {
            this.$router.push({ path: this.redirect || '/' })
            this.loading = false
          }).catch(() => {
            this.loading = false
          })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    }
  }
}
</script>

<style scoped>
.login-page {
  display: flex;
  height: 100vh;
  font-family: 'Segoe UI', sans-serif;
  background: url('https://images.unsplash.com/photo-1519125323398-675f0ddb6308?auto=format&fit=crop&w=1600&q=80') no-repeat center center;
  background-size: cover;
}

.login-form-container {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 60px;
  background-color: rgba(255, 255, 255, 0.92);
}

.login-side-image {
  flex: 1;
  background: url('https://images.unsplash.com/photo-1522204507932-cad8f286d0ba') center center/cover no-repeat;
  position: relative;
}

.overlay-card {
  position: absolute;
  bottom: 60px;
  left: 60px;
  background: rgba(255, 255, 255, 0.9);
  padding: 24px;
  border-radius: 8px;
  max-width: 300px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.quote {
  font-style: italic;
  color: #333;
  margin-bottom: 12px;
}

.author {
  font-weight: bold;
  margin-bottom: 4px;
}

.meta {
  font-size: 13px;
  color: #777;
}

.welcome-title {
  font-size: 28px;
  font-weight: bold;
  margin-bottom: 8px;
}

.subtitle {
  color: #888;
  margin-bottom: 24px;
}

.form-box {
  max-width: 400px;
}

.signin-btn {
  width: 100%;
  background-color: #111827;
  color: white;
  border-radius: 6px;
  margin-bottom: 16px;
}

.signup-tip {
  font-size: 13px;
  text-align: center;
  color: #888;
}
</style>
