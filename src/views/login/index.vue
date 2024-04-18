<template>
  <div class="login">
    <div class="login-box">
      <img src="@/assets/login/login-test2.jpeg" alt="" />
      <div class="login-form">
        <el-form ref="loginForm" :model="loginForm" :rules="loginRules">
          <div class="login-form-title">
            <img
              src="@/assets/login/yum-logo.svg"
              style="width: 447px; height: 114px"
              alt=""
            />
            <!-- <span class="title-label">YummyFood</span> -->
          </div>
          <el-form-item prop="username">
            <el-input
              v-model="loginForm.username"
              type="text"
              auto-complete="off"
              placeholder="账号"
              prefix-icon="iconfont icon-user"
            />
          </el-form-item>
          <el-form-item prop="password">
            <el-input
              v-model="loginForm.password"
              type="password"
              placeholder="密码"
              prefix-icon="iconfont icon-lock"
              @keyup.enter.native="handleLogin"
            />
          </el-form-item>
          <el-form-item style="width: 100%">
            <el-button
              :loading="loading"
              class="login-btn"
              size="medium"
              type="primary"
              style="width: 100%"
              @click.native.prevent="handleLogin"
            >
              <span v-if="!loading">登录</span>
              <span v-else>登录中...</span>
            </el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Watch } from 'vue-property-decorator'
import { Route } from 'vue-router'
import { Form as ElForm, Input } from 'element-ui'
import { UserModule } from '@/store/modules/user'
import { isValidUsername } from '@/utils/validate'

@Component({
  name: 'Login',
})
export default class extends Vue {
  private validateUsername = (rule: any, value: string, callback: Function) => {
    if (!value) {
      callback(new Error('请输入用户名'))
    } else {
      callback()
    }
  }
  private validatePassword = (rule: any, value: string, callback: Function) => {
    if (value.length < 6) {
      callback(new Error('密码必须在6位以上'))
    } else {
      callback()
    }
  }
  private loginForm = {
    username: 'admin',
    password: '123456',
  } as {
    username: String
    password: String
  }

  loginRules = {
    username: [{ validator: this.validateUsername, trigger: 'blur' }],
    password: [{ validator: this.validatePassword, trigger: 'blur' }],
  }
  private loading = false
  private redirect?: string

  @Watch('$route', { immediate: true })
  private onRouteChange(route: Route) {}

  // 登录
  private handleLogin() {
    (this.$refs.loginForm as ElForm).validate(async (valid: boolean) => {
      if (valid) {
        this.loading = true
        await UserModule.Login(this.loginForm as any)
          .then((res: any) => {
            if (String(res.code) === '1') {
              //登录成功，跳转到系统首页
              this.$router.push('/')
            } else {
              // this.$message.error(res.msg)
              this.loading = false
            }
          })
          .catch(() => {
            // this.$message.error('用户名或密码错误！')
            this.loading = false
          })
      } else {
        return false
      }
    })
  }
}
</script>

<style lang="scss">
.login {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  // background: #476dbe;
  background-color: #333;
}

.login-box {
  width: 1000px;
  height: 474.38px;
  border-radius: 8px;
  display: flex;
  img {
    width: 60%;
    height: auto;
  }
}

.title {
  margin: 0px auto 10px auto;
  text-align: left;
  color: #707070;
}

// .login-form {
//   background: #ffffff;
//   width: 40%;
//   border-radius: 0px 0px 0px 0px;
//   display: flex;
//   justify-content: center;
//   align-items: center;
//   .el-form {
//     width: 214px;
//     height: 307px;
//   }
//   .el-form-item {
//     margin-bottom: 30px;
//   }
//   .el-form-item.is-error .el-input__inner {
//     border: 0 !important;
//     border-bottom: 1px solid #65fdca !important;
//     background: #fff !important;
//   }
//   .input-icon {
//     height: 32px;
//     width: 18px;
//     margin-left: -2px;
//   }
//   .el-input__inner {
//     border: 0;
//     border-bottom: 1px solid #e9e9e8;
//     border-radius: 0;
//     font-size: 12px;
//     font-weight: 400;
//     color: #333333;
//     height: 32px;
//     line-height: 32px;
//   }
//   .el-input__prefix {
//     left: 0;
//   }
//   .el-input--prefix .el-input__inner {
//     padding-left: 26px;
//   }
//   .el-input__inner::placeholder {
//     color: #aeb5c4;
//   }
//   .el-form-item--medium .el-form-item__content {
//     line-height: 32px;
//   }
//   .el-input--medium .el-input__icon {
//     line-height: 32px;
//   }
// }
.login-form {
  background: #ffffff;
  width: 40%;
  border-radius: 0px; /* Updated border-radius for a softer look */
  display: flex;
  justify-content: center;
  align-items: center;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* New shadow for depth */
}

.el-form {
  width: 214px;
  height: 307px;
}

.el-form-item {
  margin-bottom: 30px;
}

.el-form-item.is-error .el-input__inner {
  border: 0 !important;
  border-bottom: 2px solid #0084ff !important; /* Brighter color for visibility */
  background: #fff !important;
  color: #d60000; /* New text color for error state */
}

.input-icon {
  height: 32px;
  width: 18px;
  margin-left: -2px;
}

.el-input__inner {
  border: 0;
  border-bottom: 2px solid #e9e9e8; /* Thicker bottom border for emphasis */
  border-radius: 0;
  font-size: 14px; /* Larger font size for readability */
  font-weight: 400;
  color: #333333;
  height: 32px;
  line-height: 32px;
}

.el-input__prefix {
  left: 0;
}

.el-input--prefix .el-input__inner {
  padding-left: 26px;
}

.el-input__inner::placeholder {
  color: #c2c9d6; /* Lighter placeholder for better contrast */
}

.el-form-item--medium .el-form-item__content {
  line-height: 32px;
}

.el-input--medium .el-input__icon {
  line-height: 32px;
}




// .login-btn {
//   border-radius: 17px;
//   padding: 11px 20px !important;
//   margin-top: 10px;
//   font-weight: 500;
//   font-size: 12px;
//   border: 0;
//   color: #333333;
//   // background: #09a57a;
//   background-color: #0084ff;
//   &:hover,
//   &:focus {
//     // background: #09a57a;
//     background-color: #00e1ff;
//     color: #ffffff;
//   }
// }

.login-btn {
    border-radius: 17px; /* Rounded corners */
    padding: 11px 20px !important; /* Padding for button size */
    margin-top: 10px; /* Margin from elements above */
    font-weight: 500; /* Medium weight font */
    font-size: 12px; /* Small font size */
    border: 0; /* No border */
    color: #ffffff; /* White text color for better contrast */
    background-color: #0084ff; /* Bright blue background */
    transition: all 0.3s ease; /* Smooth transition for all changes */
    cursor: pointer; /* Pointer cursor on hover */
    box-shadow: 0 2px 5px rgba(0,0,0,0.2); /* Subtle shadow for depth */

    &:hover, &:focus {
        background-color: #00e1ff; /* Lighter blue on hover/focus */
        box-shadow: 0 4px 8px rgba(0,0,0,0.3); /* Increased shadow on hover */
    }
}
.login-form-title {
  height: 36px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 40px;
  .title-label {
    font-weight: 500;
    font-size: 20px;
    color: #333333;
    margin-left: 10px;
  }
}
</style>
