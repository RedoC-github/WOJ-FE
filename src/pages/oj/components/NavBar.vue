<template>
  <div id="header">
    <Menu theme="light" mode="horizontal" @on-select="handleRoute" :active-name="activeMenu" class="oj-menu">
      <link rel="stylesheet" href="../views/fonts/font.css">
      <div class="logo" style="font-family: 'Gmarket Sans', 'sans-serif'; font-weight: bold; color: whitesmoke"><span>Wops 온라인 저지</span></div>
      <Menu-item style="font-family: 'Gmarket Sans', 'sans-serif'; font-weight: light; color: whitesmoke; color: whitesmoke" name="/">
        <Icon type="home"></Icon>
        {{$t('메인 페이지')}}
      </Menu-item>
      <Menu-item style="font-family: 'Gmarket Sans', 'sans-serif'; font-weight: light; color: whitesmoke; color: whitesmoke" name="/problem">
        <Icon type="ios-keypad"></Icon>
        {{$t('문제 페이지')}}
      </Menu-item>
      <Menu-item style="font-family: 'Gmarket Sans', 'sans-serif'; font-weight: light; color: whitesmoke; color: whitesmoke" name="/contest">
        <Icon type="trophy"></Icon>
        {{$t('컨테스트')}}
      </Menu-item>
      <Menu-item style="font-family: 'Gmarket Sans', 'sans-serif'; font-weight: light; color: whitesmoke; color: whitesmoke" name="/status">
        <Icon type="ios-pulse-strong"></Icon>
        {{$t('채점 현황')}}
      </Menu-item>
      <Submenu style="font-family: 'Gmarket Sans', 'sans-serif'; font-weight: light; color: whitesmoke; color: whitesmoke" name="rank">
        <template slot="title">
          <Icon type="podium"></Icon>
          {{$t('랭킹')}}
        </template>
        <Menu-item name="/acm-rank">
          {{$t('ACM 랭크')}}
        </Menu-item>
        <Menu-item name="/oi-rank">
          {{$t('OI 랭크')}}
        </Menu-item>
      </Submenu>
      <Submenu style="font-family: 'Gmarket Sans', 'sans-serif'; font-weight: light; color: whitesmoke; color: whitesmoke" name="about">
        <template slot="title">
          <Icon type="information-circled"></Icon>
          {{$t('m.About')}}
        </template>
        <Menu-item name="/about">
          {{$t('저지 시스템')}}
        </Menu-item>
        <Menu-item name="/FAQ">
          {{$t('m.FAQ')}}
        </Menu-item>
      </Submenu>
      <template v-if="!isAuthenticated">
        <div class="btn-menu">
          <Button type="ghost"
                  ref="loginBtn"
                  shape="circle"
                  @click="handleBtnClick('login')">{{$t('로그인(영문)')}}
          </Button>
          <Button v-if="website.allow_register"
                  type="ghost"
                  shape="circle"
                  @click="handleBtnClick('register')"
                  style="margin-left: 5px;">{{$t('등록(영문)')}}
          </Button>
        </div>
      </template>
      <template v-else>
        <Dropdown class="drop-menu" @on-click="handleRoute" placement="bottom" trigger="click">
          <Button type="text" class="drop-menu-title">{{ user.username }}
            <Icon type="arrow-down-b"></Icon>
          </Button>
          <Dropdown-menu slot="list">
            <Dropdown-item name="/user-home">{{$t('내 페이지')}}</Dropdown-item>
            <Dropdown-item name="/status?myself=1">{{$t('내 제출')}}</Dropdown-item>
            <Dropdown-item name="/setting/profile">{{$t('설정')}}</Dropdown-item>
            <Dropdown-item v-if="isAdminRole" name="/admin">{{$t('관리')}}</Dropdown-item>
            <Dropdown-item divided name="/logout">{{$t('로그아웃')}}</Dropdown-item>
          </Dropdown-menu>
        </Dropdown>
      </template>
    </Menu>
    <Modal v-model="modalVisible" :width="400">
      <div slot="header" class="modal-title">{{$t('m.Welcome_to')}} {{website.website_name_shortcut}}</div>
      <component :is="modalStatus.mode" v-if="modalVisible"></component>
      <div slot="footer" style="display: none"></div>
    </Modal>
  </div>
</template>

<script>
  import { mapGetters, mapActions } from 'vuex'
  import login from '@oj/views/user/Login'
  import register from '@oj/views/user/Register'

  export default {
    components: {
      login,
      register
    },
    mounted () {
      this.getProfile()
    },
    methods: {
      ...mapActions(['getProfile', 'changeModalStatus']),
      handleRoute (route) {
        if (route && route.indexOf('admin') < 0) {
          this.$router.push(route)
        } else {
          window.open('/admin/')
        }
      },
      handleBtnClick (mode) {
        this.changeModalStatus({
          visible: true,
          mode: mode
        })
      }
    },
    computed: {
      ...mapGetters(['website', 'modalStatus', 'user', 'isAuthenticated', 'isAdminRole']),
      activeMenu () {
        return '/' + this.$route.path.split('/')[1]
      },
      modalVisible: {
        get () {
          return this.modalStatus.visible
        },
        set (value) {
          this.changeModalStatus({visible: value})
        }
      }
    }
  }
</script>

<style lang="less" scoped>
  #header {
    min-width: 300px;
    position: fixed;
    top: 0;
    left: 0;
    height: auto;
    width: 100%;
    z-index: 1000;
    background-color: #111111;
    box-shadow: 0 1px 5px 0 rgba(0, 0, 0, 0.1);
    .oj-menu {
      background: #111111;
    }

    .logo {
      margin-left: 2%;
      margin-right: 2%;
      font-size: 20px;
      float: left;
      line-height: 60px;
    }

    .drop-menu {
      float: right;
      margin-right: 30px;
      position: absolute;
      right: 10px;
      &-title {
        font-size: 18px;
      }
    }
    .btn-menu {
      font-size: 16px;
      float: right;
      margin-right: 10px;
    }
  }

  .modal {
    &-title {
      font-size: 18px;
      font-weight: 600;
    }
  }
</style>
