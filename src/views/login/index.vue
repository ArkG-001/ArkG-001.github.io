<template>
  <div class="farther">
    <div id="iviewUi" />
    <div class="login-container">
      <el-form ref="loginForm" :model="loginForm" :rules="loginRules" class="login-form" auto-complete="on" label-position="left">

        <div class="title-container">
          <h3 class="title">Login Form</h3>
        </div>

        <el-form-item prop="username">
          <span class="svg-container">
            <svg-icon icon-class="user" />
          </span>
          <el-input
            ref="username"
            v-model="loginForm.username"
            placeholder="Username"
            name="username"
            type="text"
            tabindex="1"
            auto-complete="on"
          />
        </el-form-item>

        <el-form-item prop="password">
          <span class="svg-container">
            <svg-icon icon-class="password" />
          </span>
          <el-input
            :key="passwordType"
            ref="password"
            v-model="loginForm.password"
            :type="passwordType"
            placeholder="Password"
            name="password"
            tabindex="2"
            auto-complete="on"
            @keyup.enter.native="handleLogin"
          />
          <span class="show-pwd" @click="showPwd">
            <svg-icon :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'" />
          </span>
        </el-form-item>

        <el-button :loading="loading" type="primary" style="width:100%;margin-bottom:30px;" @click.native.prevent="handleLogin">Login</el-button>

        <div class="tips">
          <span style="margin-right:20px;">username: admin</span>
          <span> password: any</span>
        </div>

      </el-form>
    </div>
  </div>
</template>

<script>
import { validUsername } from '@/utils/validate'
// 引入three.js
import * as THREE from 'three'

export default {
  name: 'Login',
  props: {
    amountX: {
      type: Number,
      default: 50
    },
    amountY: {
      type: Number,
      default: 50
    },
    // 控制点颜色
    color: {
      type: String,
      default: '#097bdb'
    },
    top: {
      type: Number,
      default: 350
    }
  },
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
        username: [
          { required: true, trigger: 'blur', validator: validateUsername }
        ],
        password: [
          { required: true, trigger: 'blur', validator: validatePassword }
        ]
      },
      loading: false,
      passwordType: 'password',
      redirect: undefined,
      count: 0,
      // 用来跟踪鼠标水平位置
      mouseX: 0,
      windowHalfX: null,
      // 相机
      camera: null,
      // 场景
      scene: null,
      // 批量管理粒子
      particles: null,
      // 渲染器
      renderer: null
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
  mounted() {
    this.init()
    this.animate()
  },
  methods: {
    init: function() {
      const SEPARATION = 100
      const SCREEN_WIDTH = window.innerWidth
      const SCREEN_HEIGHT = window.innerHeight
      const container = document.createElement('div')
      this.windowHalfX = window.innerWidth / 2
      container.style.position = 'relative'
      container.style.top = `${this.top}px`
      container.style.height = `${SCREEN_HEIGHT - this.top}px`
      document.getElementById('iviewUi').appendChild(container)

      this.camera = new THREE.PerspectiveCamera(
        75,
        SCREEN_WIDTH / SCREEN_HEIGHT,
        1,
        10000
      )
      this.camera.position.z = 1000

      this.scene = new THREE.Scene()

      const numParticles = this.amountX * this.amountY
      const positions = new Float32Array(numParticles * 3)
      const scales = new Float32Array(numParticles)
      // 初始化粒子位置和大小
      let i = 0
      let j = 0
      for (let ix = 0; ix < this.amountX; ix++) {
        for (let iy = 0; iy < this.amountY; iy++) {
          positions[i] = ix * SEPARATION - (this.amountX * SEPARATION) / 2
          positions[i + 1] = 0
          positions[i + 2] = iy * SEPARATION - (this.amountY * SEPARATION) / 2
          scales[j] = 1
          i += 3
          j++
        }
      }

      const geometry = new THREE.BufferGeometry()
      geometry.setAttribute(
        'position',
        new THREE.BufferAttribute(positions, 3)
      )
      geometry.setAttribute('scale', new THREE.BufferAttribute(scales, 1))
      // 初始化粒子材质
      const material = new THREE.ShaderMaterial({
        uniforms: {
          color: { value: new THREE.Color(this.color) }
        },
        vertexShader: `
          attribute float scale;
          void main() {
            vec4 mvPosition = modelViewMatrix * vec4( position, 2.0 );
            gl_PointSize = scale * ( 300.0 / - mvPosition.z );
            gl_Position = projectionMatrix * mvPosition;
          }
        `,
        fragmentShader: `
          uniform vec3 color;
          void main() {
            if ( length( gl_PointCoord - vec2( 0.5, 0.5 ) ) > 0.475 ) discard;
            gl_FragColor = vec4( color, 1.0 );
          }
        `
      })

      this.particles = new THREE.Points(geometry, material)
      this.scene.add(this.particles)

      this.renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true })
      this.renderer.setSize(container.clientWidth, container.clientHeight)
      this.renderer.setPixelRatio(window.devicePixelRatio)
      this.renderer.setClearAlpha(0)
      container.appendChild(this.renderer.domElement)

      window.addEventListener('resize', this.onWindowResize, {
        passive: false
      })
      document.addEventListener('mousemove', this.onDocumentMouseMove, {
        passive: false
      })
      document.addEventListener('touchstart', this.onDocumentTouchStart, {
        passive: false
      })
      document.addEventListener('touchmove', this.onDocumentTouchMove, {
        passive: false
      })
    },
    render: function() {
      this.camera.position.x += (this.mouseX - this.camera.position.x) * 0.05
      this.camera.position.y = 400
      this.camera.lookAt(this.scene.position)
      const positions = this.particles.geometry.attributes.position.array
      const scales = this.particles.geometry.attributes.scale.array
      // 计算粒子位置及大小
      let i = 0
      let j = 0
      for (let ix = 0; ix < this.amountX; ix++) {
        for (let iy = 0; iy < this.amountY; iy++) {
          positions[i + 1] =
            Math.sin((ix + this.count) * 0.3) * 100 +
            Math.sin((iy + this.count) * 0.5) * 100
          scales[j] =
            (Math.sin((ix + this.count) * 0.3) + 1) * 8 +
            (Math.sin((iy + this.count) * 0.5) + 1) * 8
          i += 3
          j++
        }
      }
      // 重新渲染粒子
      this.particles.geometry.attributes.position.needsUpdate = true
      this.particles.geometry.attributes.scale.needsUpdate = true
      this.renderer.render(this.scene, this.camera)
      this.count += 0.1
    },
    animate: function() {
      requestAnimationFrame(this.animate)
      this.render()
    },
    onDocumentMouseMove: function(event) {
      this.mouseX = event.clientX - this.windowHalfX
    },
    onDocumentTouchStart: function(event) {
      if (event.touches.length === 1) {
        this.mouseX = event.touches[0].pageX - this.windowHalfX
      }
    },
    onDocumentTouchMove: function(event) {
      if (event.touches.length === 1) {
        event.preventDefault()
        this.mouseX = event.touches[0].pageX - this.windowHalfX
      }
    },
    onWindowResize: function() {
      this.windowHalfX = window.innerWidth / 2
      this.camera.aspect = window.innerWidth / window.innerHeight
      this.camera.updateProjectionMatrix()
      this.renderer.setSize(window.innerWidth, window.innerHeight)
    },
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
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
          this.loading = true
          this.$store
            .dispatch('user/login', this.loginForm)
            .then(() => {
              this.$router.push({ path: this.redirect || '/' })
              this.loading = false
            })
            .catch(() => {
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

<style lang="scss">
/* 修复input 背景不协调 和光标变色 */
/* Detail see https://github.com/PanJiaChen/vue-element-admin/pull/927 */

$bg: #283443;
$light_gray: #fff;
$cursor: #fff;

@supports (-webkit-mask: none) and (not (cater-color: $cursor)) {
  .login-container .el-input input {
    color: $cursor;
  }
}

/* reset element-ui css */
.login-container {
  .el-input {
    display: inline-block;
    height: 47px;
    width: 85%;

    input {
      background: transparent;
      border: 0px;
      -webkit-appearance: none;
      border-radius: 0px;
      padding: 12px 5px 12px 15px;
      color: $light_gray;
      height: 47px;
      caret-color: $cursor;

      &:-webkit-autofill {
        box-shadow: 0 0 0px 1000px $bg inset !important;
        -webkit-text-fill-color: $cursor !important;
      }
    }
  }

  .el-form-item {
    border: 1px solid rgba(255, 255, 255, 0.1);
    background: rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    color: #454545;
  }
}
</style>

<style lang="scss" scoped>
$bg: #2d3a4b;
$dark_gray: #889aa4;
$light_gray: #eee;
.farther {
  height: 100%;
  width: 100%;
  position: relative;
}
#iviewUi {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  // background-color: green;
  z-index: 1000000000;

}
.login-container {
  // background-color: pink;
  background-image: url(../../assets/main/login_bg.png);
  height: 100%;
  position: relative;
  // width: 100%;
  // background-color: $bg;
  // overflow: hidden;

  .login-form {
    // position: relative;
    // position: absolute;
    // top: 50%;
    // left: 50%;
    // transform: translate(-50%,-50%);

    width: 520px;
    max-width: 100%;
    padding: 160px 35px 0;
    margin: 0 auto;
    overflow: hidden;
  }

  .tips {
    font-size: 14px;
    color: #fff;
    margin-bottom: 10px;

    span {
      &:first-of-type {
        margin-right: 16px;
      }
    }
  }

  .svg-container {
    padding: 6px 5px 6px 15px;
    color: $dark_gray;
    vertical-align: middle;
    width: 30px;
    display: inline-block;
  }

  .title-container {
    position: relative;

    .title {
      font-size: 26px;
      color: $light_gray;
      margin: 0px auto 40px auto;
      text-align: center;
      font-weight: bold;
    }
  }

  .show-pwd {
    position: absolute;
    right: 10px;
    top: 7px;
    font-size: 16px;
    color: $dark_gray;
    cursor: pointer;
    user-select: none;
  }
}
</style>
