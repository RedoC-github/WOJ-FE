<template>
  <div>
    <link rel="stylesheet" href="../fonts/font.css">
    <panel :padding="15" class="container"  style="font-family: 'Gmarket Sans', 'sans-serif'">
      <div slot="title">{{$t('결과')}}</div>
      <div class="content">
        <ul>
          <li><b>{{$t('m.Pending')}} & {{$t('m.Judging')}}</b> : {{$t('대기중')}}</li>
          <li><b>{{$t('m.Compile_Error')}}</b> :	{{$t('컴파일 에러')}}
      </li>
          <li><b>{{$t('m.Accepted')}}</b> :	{{$t('맞았습니다!')}}</li>
          <li><b>{{$t('m.Wrong_Answer')}}</b> :	{{$t('틀렸습니다.')}}</li>
          <li>
            <b>{{$t('m.Runtime_Error')}}</b>
            :	{{$t('0으로 나누는 등 런타임 에러')}}
          </li>
          <li><b>{{$t('m.Time_Limit_Exceeded')}}</b>
            :	{{$t('시간 제한 초과')}}
          </li>
          <li><b>{{$t('m.Memory_Limit_Exceeded')}}</b> :	{{$t('메모리 초과(메모리 누수를 점검하세요)')}}</li>
          <li><b>{{$t('m.System_Error')}}</b> :	{{$t('시스템 오류(이 경우에는 담당자에게 연락하세요)')}}
          </li>
        </ul>
      </div>
    </panel>

  </div>
</template>

<script>
  import utils from '@/utils/utils'

  export default {
    data () {
      return {
        languages: []
      }
    },
    beforeRouteEnter (to, from, next) {
      utils.getLanguages().then(languages => {
        next(vm => {
          vm.languages = languages
        })
      })
    }
  }
</script>

<style scoped lang="less">
  .container {
    margin-bottom: 20px;

    .content {
      font-size: 16px;
      margin: 0 50px 20px 50px;
      > ul {
        list-style: disc;
        li {
          line-height: 2;
          .title {
            font-weight: 500;
          }
        }
      }
    }
  }
</style>
