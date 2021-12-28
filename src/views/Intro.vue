<template>
  <div id="intro">
    <h1><img :src="require(`@/assets/img/img_logo.png`)" alt="라이나 생명 고연령 건강빙고" /></h1>
    <ul class="visual" ref="interval">
      <li>
        <div>
          <span style="--i:0.5">아프기보다는 <strong>내 인생을<br>즐기고 싶습니다.</strong></span>
        </div>
      </li>

      <li>
        <div>
          <span style="--i:0.5">자식에게<br>짐이 되기 보다는 <strong>도움이 되고 싶습니다.</strong></span>
        </div>
      </li>

      <li>
        <div>
          <span style="--i:0.5">이런 마음,<br>누구나 다 똑같지 않나요?</span>
          <p style="--i:2">
            <strong>건강한 삶은<br> 생활습관에서부터 시작합니다.<br></strong>
            <span>지금, 빙고로 평소 생활습관을 체크하고 <br>맞춤보장까지 알아 보세요!</span>
          </p>

          <router-link to="/home" style="--i:2">
            내일을 바꾸는 건강생활 빙고<br><span>시작하기</span>
          </router-link>

          <em style="--i:2">라이나생명 준법감시인 확인필 제 2021-M00674-02호(2021-10-15)</em>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  mounted() {
    this.intervalView();
  },
  methods: {
    intervalView(){
      var _target = this.$refs.interval.children;
      _target.forEach((element,index) => {
        setTimeout(() => {
          // siblings class
          Array.from(_target).filter(e => e != element ? e.classList.remove('on') : e.classList.add('on'));
        }, 3500 * index);
      });
    },
  },
}
</script>