<template>
  <div id="test">
    <h1><img :src="require(`@/assets/img/img_logo.png`)" alt="라이나 생명 고연령 건강빙고" /></h1>
    <h2>내일을 바꾸는 <span>건강생활 빙고!</span></h2>
    <p :class="{on : resultPage}">
      <span>나에게 해당되는 빙고판을 선택하세요.</span>
      <span>※ 해당 결과는 생활습관으로 필요한 보장을 예측하는 진단기로,<br>정확한 질병 진단은 병원에서 받으셔야 합니다.</span>
    </p>

    <!-- contents -->
    <div
      class="contents"
      :class="{on : resultPage}"
    >
      <!-- puzzle-box -->
      <div class="puzzle-box">
        <div
          class="row"
          v-for="(row, rowIndex) in bingoList"
          :key="`bingo-row-${rowIndex}`"
        >
          <div
            class="piece noApp"
            v-for="(item, itemIndex) in row"
            :key="`bingo-item-${itemIndex}`"
            :id="`td-${rowIndex}-${itemIndex}`"
            @click="clickItem(rowIndex, itemIndex)"
          >
            <span v-html="item"></span>
          </div>
        </div>
      </div>
      <!-- explan-wrap -->
      <div class="explan-wrap" ref="explanWrap">
        <button
          type="button"
          class="result-btn noApp"
          @click="showResult()"
          v-if="!resultPage"
        >
          <span>결과보기</span>
        </button>

        <div class="result-page" v-show="resultPage">
          <div class="countBox">
            <ul>
              <li><strong>선택</strong> <em>{{ selectItem }}</em><span>개</span></li>
              <li><strong>완성</strong> <em>{{ bingo }}</em><span>줄</span></li>
            </ul>
          </div>

          <div class="infoBox"
            v-for="item in bottomSectionArr"
            :key="item"
            v-html="item"
          >
          </div>

          <div class="guaranteeBox">
            <div class="inner">
              <h3>나에게 필요한 <strong>보장</strong>은?</h3>
              <p><span>버튼</span>을 눌러 상품을 자세히 확인해 보세요.</p>
              <ul ref="guaranteeBox">
                <li
                  v-for="(guarantee, index) in guaranteeArr"
                  :key="`guarantee-${index}`"
                  :class="{on: guarantee.active, active: currentTabMenu == index}"
                  @click="onClickGuarantee(index)"
                >
                  <p>
                    <span></span>
                    {{guarantee.title}}
                  </p>
                </li>
              </ul>
            </div>

            <div class="movie">
              <div v-html="guaranteeArr[currentTabMenu].text"></div>
              <button type="button" @click="onClickMovie(currentTabMenu)"><span>상품 알아보기</span></button>
            </div>
          </div>

          <div class="infoBox2" ref="infoBox2">
            <strong>보험이 궁금할 때,<br>케어가 필요할 때!</strong>
            <ul>
              <li><p>간편하게 <strong>전화 상담</strong> 가능!</p></li>
              <li><p>쉽고 빠른 <strong>채팅 상담</strong> 가능!</p></li>
            </ul>
            <p>보험 상담 전화번호와 채팅 방법은<br>문자메시지에서 확인하세요!</p>
          </div>

          <div class="shareBox">
            <div @click="onClickPopup(1)">
              <p>남녀별로 조심해야 하는<br> 질병이 달라요!</p>
            </div>
            <!-- <div @click="onClickPopup(2)"><p><span>친구에게 보내보세요!</span>공유하기</p></div> -->
          </div>

          <div class="replayBox">
            <button type="button" @click="replayBingo()"><span>처음으로</span></button>
          </div>
        </div>
      </div>
    </div>
    <!-- //contents -->

    <!-- POPUP share -->
    <transition name="popupDimmed" mode="out-in">
    <div id="layerPopupBox" v-if="popupToggle">
      <div class="innerBox">
          <transition name="popupDimmed" mode="out-in">
            <div v-if="diseasePopup">
              <h2>남녀별 조심해야 하는 질병</h2>
              <div class="scrollBox">
                <img src="~@/assets/img/test/img_popup.png" alt="">
              </div>
            </div>
          </transition>

          <transition name="popupDimmed" mode="out-in">
            <div v-if="sharePopup">
              <h2>공유하기</h2>
              <div class="scrollBox auto">
                <ul class="share-list">
                  <li @click="onClickKakao()">
                    <img src="~@/assets/img/btn_kakao.png" alt="">
                    <span>카카오톡</span>
                  </li>
                  <li
                    class="url-copy"
                    @click="onClickShare()"
                  >
                  <img src="~@/assets/img/btn_sms.png" alt="">
                  <span>문자</span>
                  </li>
                </ul>
              </div>
            </div>
          </transition>
        <button type="button" @click="onClickPopup()">닫기</button>
      </div>
    </div>
    </transition>
    <!-- //POPUP -->
  </div>
</template>

<script>
import smoothscroll from 'smoothscroll-polyfill';
smoothscroll.polyfill();

export default {
  name: "Home",
  components: {},
  data() {
    return {
      resultPage: false,
      popupToggle: false,
      diseasePopup: false,
      sharePopup: false,
      guaranteeArr: [
        {
          active: false,
          title: '암',
          text: '<p>나이가 많아도, 약을 먹고 있어도<br>가입이 가능한 <strong>암보험</strong></p>',
          url: 'https://www.youtube.com/watch?v=4vIqRQUbMbA'
        },
        {
          active: false,
          title: '질병',
          text: '<p>고령자, 유병력자에게 꼭 필요한<br> <strong>건강보험</strong></p>',
          url: 'https://www.lina.co.kr/product/productView/productView_P00230.htm'
        },
        {
          active: false,
          title: '심뇌혈관',
          text: '<p>어느 날 갑자기?<br> 나이 들수록 꼭 필요한<br><strong>심뇌혈관보험</strong></p>',
          url: 'https://www.lina.co.kr/product/productView/productView_P00172.htm'
        },
        {
          active: false,
          title: '치매',
          text: '<p>경증치매 진단금부터<br> 간병비까지 챙겨주는<br> <strong>치매보험</strong></p>',
          url: 'https://www.youtube.com/watch?v=5V8E65ImgEs'
        },
        {
          active: false,
          title: '치아',
          text: '<p>부담되는 임플란트,<br> 자주하는 충전치료까지<br>가능한 <strong>치아보험</strong></p>',
          url: 'https://www.youtube.com/watch?v=NSN4c10YJGM​'
        },
      ],
      currentTabMenu: 0,
      bingo: 0,
      selectItem: 0,
      sectionArr: [],
      bottomSectionArr: [],
      bingoList: [
        [
          "암<br>가족력이<br> 있다",
          "운동을<br>규칙적으로<br>하지 않는다",
          "정기적으로<br>치과에<br>가지 않는다",
          "당뇨나<br>혈압이<br>있다",
        ],
        [
          "65세<br>이상이다",
          "하루 평균<br>술은<br>2잔 이상<br>마신다",
          "수면시간이<br>부족하다",
          "혼자<br>있는 것을<br>좋아한다",
        ],
        [
          "과체중이다",
          "햄이나<br>구운 고기를<br>좋아한다",
          "담배를<br>피고 있거나<br>핀적이 있다",
          "앉아있는<br>시간이 많다",
        ],
        [
          "과식하거나<br>음식을<br>빨리 먹는다",
          "콜레스테롤<br>수치가<br>높다",
          "만성질환을<br>보유하고<br>있다",
          "스트레스는<br>따로 풀지<br>않는다",
        ],
      ],
      selectBingo: [
        [0, 1, 2, 3],
        [4, 5, 6, 7],
        [8, 9, 10, 11],
        [12, 13, 14, 15],
      ],
      resultDiseaseTxt: [
        {txt: "암 발병 위험에 각별한 주의가 필요합니다."},
        {txt: "심장 뇌혈관질환에 대한 각별한 주의가 필요합니다."},
        {txt: "세상에서 가장 슬픈 병 치매,<br>치매예방이 필요합니다."},
        {txt: "과거 병력이나 혹은 나이때문에<br>보험가입을 망설이셨다면<br> 간편심사 상품을 알아보세요!"},
      ],
      resultAdvice: [
        {advice: "<p>올바른 생활습관 관리가<br><strong>시급합니다.</strong></p> <span>오늘부터 꾸준한 운동과 올바른 생활습관으로 건강관리를 시작하세요!<br>더불어 3대 사망원인 질병과 치매, 치아까지 대비하는 것이 좋습니다.</span>"},
        {advice: "<p><strong>좋지 않은 생활습관들</strong>을<br> 가지고 계시네요.</p> <span>작은 습관들이 모여 큰 병을 만들 수 있습니다.<br>습관 개선과 함께 필수적으로 준비해야 할 보장도 챙겨보세요.</span>"},
        {advice: "<p>지금은 <strong>백세 시대!</strong></p> <span>비교적 건강한 생활습관을 가지고 계시네요.<br>혹시 모를 미래, 더 든든하시라고 백세시대 필수 보장설계를 추천드려요!</span>"},
        {advice: "<p>조금 <strong>주의</strong>가 필요해요.</p> <span>꾸준한 운동과 올바른 생활습관으로 건강을 챙기셔야 합니다.<br>더불어 든든한 보장도 미리미리 준비하세요!</span>"},
      ],
      healthGuide: [
        {txt: "<p>건강한 생활습관 가이드 </p><ul><li>적정 체중과 적정 체지방량을 유지한다.</li><li>전곡류와 콩류를 많이 먹는다.</li><li>여러가지 채소와 과일을 먹는다.</li><li>붉은색 육류를 적게 먹는다.</li><li>염분을 줄이고 싱겁게 먹는다.</li><li>저지방 우유를 매일 한 컵 마신다.</li></ul>"},
      ],
      isKakaoSamsung: false,
    };
  },
  mounted() {

  },
  created() {
    window.addEventListener('scroll', this.scrollEvent);
  },
  destroyed() {
    window.removeEventListener('scroll', this.scrollEvent);
  },
  methods: {
    scrollEvent(){
      var _target = this.$refs.infoBox2,
          _trigger = window.pageYOffset + window.outerHeight,
          _scroll = _target.offsetTop + window.outerHeight/2;

      if( _trigger > _scroll && !_target.classList.contains('on') ){
        _target.classList.add('on');
      }
    },
    clickItem(rowIndex, itemIndex) {
      if (this.resultPage) {
        console.log("결과 페이지 출력 중~ 클릭 이벤트 무시");
        return;
      }

      var addValue = 4 * rowIndex + itemIndex;            // 누른 아이템 인덱스 (0 ~ 24)
      var index = this.selectBingo[rowIndex][itemIndex];  // 누른 아이템 인덱스 (0 ~ 24, 두번 누르면 -1)

      if (index == -1) {
        console.log('-1');
        this.onAddSelectTd(rowIndex, itemIndex);
        this.$set(this.selectBingo[rowIndex], itemIndex, addValue);
        //눌러진 아이템 다시 누르면 원래 넘버로 채우기
        return
      }

      this.onAddSelectTd(rowIndex, itemIndex);
      this.$set(this.selectBingo[rowIndex], itemIndex, -1);
      // 클릭하면 addClass 해주고 넘버링 -1 빙고배열에 넣어 줌
    },
    /**
     * 선택한 빙고 숫자 저장
     */
    selectedItemNum() {
      this.selectItem = 0;
      for (let i = 0; i < 4; i++) {
        for (let j = 0; j < 4; j++) {
          if (this.selectBingo[i][j] == -1) {
            this.selectItem += 1;
          }
        }
      }
    },
    /**
     * rowIndex, itemIndex에 맞는 빙고에 class 추가(td-0-0), 선택한 빙고 숫자 저장
     */
    onAddSelectTd(rowIndex, itemIndex) {
      this.$nextTick(() => {
        document.getElementById(`td-${rowIndex}-${itemIndex}`).classList.toggle("on");
        this.selectedItemNum();
      });
    },
    showResult() {
      // 결과보기 버튼
      this.bingo = 0;
      this.resultPage = !this.resultPage;
      this.checkHorizontal();     // 가로줄 5,6,7,8 | 9,10,11,12
      this.checkVertical();       // 세로줄 2,6,10,14
      this.checkDiagonalLeft();   // 대각선 1,6,11,16
      this.checkDiagonalRight();  // 대각선 4,7,10,13
      this.checkInsurance();      // 질병 4,5,15 | 치아 3,6,7,11,16
      this.section03Txt();        // 완성된 빙고 줄 숫자에 따른 분기
      this.noChecked();

      this.$nextTick(() => {
        // var scrollValue = this.$refs.explanWrap.getBoundingClientRect().top + window.pageYOffset -5; 
        // window.scrollTo({ top: scrollValue, behavior: "smooth" })
      });
    },
    /**
     * 세로줄 2,6,10,14
     */
    checkVertical() {
      for (let i = 0; i < 4; i++) {
        for (let j = 0; j < 4; j++) {
          if (this.selectBingo[j][i] === -1) {
            if (j === 3) {
              for (let k = 0; k < 4; k++) {
                if (k == 3 && i == 1) {
                  this.sectionIconAdd("cardiovascular");
                  if ( !this.sectionArr.includes("심장 뇌혈관질환에 대한 각별한 주의가 필요합니다.") ) {
                    this.sectionArr.push(this.resultDiseaseTxt[1].txt);
                  }
                }
                document.getElementById(`td-${k}-${i}`).classList.add("bingo");
              }
              this.bingo++;
            }
          } else {
            break;
          }
        }
      }
    },
    /**
     * 가로줄 5,6,7,8 | 9,10,11,12
     */
    checkHorizontal() {
      for (let i = 0; i < 4; i++) {
        if ( this.selectBingo[i].every(x => {return x === -1}) ) {
          for (let j = 0; j < 4; j++) {
            document.getElementById(`td-${i}-${j}`).classList.add("bingo");
            if (i == 1 && j == 3) {
              // 5,6,7,8 치매
              this.sectionArr.push(this.resultDiseaseTxt[2].txt);
              this.sectionIconAdd("alzheimer");
            } else if (i == 2 && j == 3) {
              // 9,10,11,12 심뇌혈관
              this.sectionArr.push(this.resultDiseaseTxt[1].txt);
              this.sectionIconAdd("cardiovascular");
            }
          }
          this.bingo++;
        }
      }
    },
    /**
     * 대각선 1,6,11,16
     */
    checkDiagonalLeft() {
      var check = true;
      for (let i = 0; i < 4; i++) {
        if (this.selectBingo[i][i] !== -1) {
          check = false;
        }
      }
      if (check === true) {
        for (let i = 0; i < 4; i++) {
          if (this.selectBingo[i][i] == -1) {
            document.getElementById(`td-${i}-${i}`).classList.add("bingo");
          }
        }
        this.sectionArr.push(this.resultDiseaseTxt[0].txt);
        this.bingo++;
      }
      this.sectionIconAdd("cancer");
    },
    /**
     * 대각선 4,7,10,13
     */
    checkDiagonalRight() {
      var check = true;

      for (let i = 0; i < 4; i++) {
        if (this.selectBingo[i][3 - i] !== -1) {
          check = false;
        }
      }

      if (check === true) {
        for (let i = 0; i < 4; i++) {
          document.getElementById(`td-${i}-${3 - i}`).classList.add("bingo");
        }
        if ( !this.sectionArr.includes("암 발병 위험에 각별한 주의가 필요합니다.") ) {
          this.sectionArr.push(this.resultDiseaseTxt[0].txt);
        }
        this.bingo++;
      }
      this.sectionIconAdd("cancer");
    },
    /**
     * 완성된 빙고 줄 숫자에 따른 분기
     */
    section03Txt() {
      if (this.bingo > 3) {
        // 4줄 이상
        this.bottomSectionArr.push(this.resultAdvice[0].advice);
        this.sectionIconAdd("cancer/health/cardiovascular/alzheimer");
      } else if (this.bingo > 0 && this.bingo < 4) {
        // 1~3줄
        this.bottomSectionArr.push(this.resultAdvice[1].advice);
        this.sectionIconAdd("cancer/health/cardiovascular");
      } else if (this.selectItem < 6 && this.bingo == 0) {
        // 완성빙고 없음, 선택항목수 6개 미만
        this.bottomSectionArr.push(this.resultAdvice[2].advice);
        this.sectionIconAdd("cancer");
      } else if (this.bingo == 0 && this.selectItem >= 6) {
        console.log(this.resultAdvice[2].advice);
        // 완성빙고 없음, 선택항목수 6개 이상
        this.bottomSectionArr.push(this.resultAdvice[3].advice);
        this.sectionIconAdd("cancer/cardiovascular");
      }
    },
    /**
     * 질병 4,5,15 | 치아 3,6,7,11,16
     */
    checkInsurance() {
      // 질병 4,5,15
      if (
        this.selectBingo[0][3] == -1 ||
        this.selectBingo[1][0] == -1 ||
        this.selectBingo[3][2] == -1
      ) {
        if (!this.sectionArr.includes(this.resultDiseaseTxt[3].txt))
          this.sectionArr.push(this.resultDiseaseTxt[3].txt);
          this.sectionIconAdd("health");
      }
      // 치아 3,6,7,11,16
      if (
        this.selectBingo[0][2] == -1 ||
        this.selectBingo[1][1] == -1 ||
        this.selectBingo[1][2] == -1 ||
        this.selectBingo[2][2] == -1 ||
        this.selectBingo[3][3] == -1
      ) {
        if (!this.sectionArr.includes(this.resultDiseaseTxt[3].txt))
          this.sectionArr.push(this.resultDiseaseTxt[3].txt);
          this.sectionIconAdd("teeth");
      }
    },
    noChecked() {
      if (this.sectionArr.length == 0) {
        this.sectionArr.push(this.healthGuide[0].txt);
      }
    },
    sectionIconAdd(desease) {
      switch (desease) {
        case "cancer":
          this.guaranteeArr[0].active = true;
          break;
        case "health":
          this.guaranteeArr[1].active = true;
          break;
        case "cardiovascular":
          this.guaranteeArr[2].active = true;
          break;
        case "alzheimer":
          this.guaranteeArr[3].active = true;
          break;
        case "teeth":
          this.guaranteeArr[4].active = true;
          break;
        case "cancer/health/cardiovascular/alzheimer":
          this.guaranteeArr[0].active = true;
          this.guaranteeArr[1].active = true;
          this.guaranteeArr[2].active = true;
          this.guaranteeArr[3].active = true;
          break;
        case "cancer/health/cardiovascular":
          this.guaranteeArr[0].active = true;
          this.guaranteeArr[1].active = true;
          this.guaranteeArr[2].active = true;
          break;
        case "cancer/cardiovascular":
          this.guaranteeArr[0].active = true;
          this.guaranteeArr[2].active = true;
          break;
      }
    },
    replayBingo() {
      window.scrollTo({ top: 0});
      this.resultPage = false;
      this.guaranteeArr.forEach(element => element.active = false);
      this.bingo = 0;
      this.selectItem = 0;
      this.currentTabMenu = 0;
      this.sectionArr = [];
      this.bottomSectionArr = [];
      for (var i = 0; i < 4; i++) {
        for (var j = 0; j < 4; j++) {
          document.getElementById(`td-${i}-${j}`).classList.remove("bingo");
          document.getElementById(`td-${i}-${j}`).classList.remove("on");

          if (i == 0) {
            this.selectBingo[i][j] = j;
          } else if (i == 1) {
            this.selectBingo[i][j] = j + 4;
          } else if (i == 2) {
            this.selectBingo[i][j] = j + 8;
          } else if (i == 3) {
            this.selectBingo[i][j] = j + 12;
          }
        }
      }
    },
    onClickPopup(arg){
      console.log(arg);
      document.body.classList.toggle('on');
      this.popupToggle = !this.popupToggle;
      if(arg == 1){
        this.diseasePopup = true;
        this.sharePopup = false;
      } else if(arg == 2){
        this.diseasePopup = false;
        this.sharePopup = true;
      } else {
        this.diseasePopup = false;
        this.sharePopup = false;
      }
      
    },
    onClickKakao(){
      Kakao.Link.sendCustom({
        templateId: 63933,
        templateArgs: {
          'title': '내일을 바꾸는 건강생활 빙고',
          'description': '빙고로 평소 생활습관을 체크하고 맞춤보장까지 알아보세요!'
        }
      });
    },
    copyToClipboard(val) {
        const t = document.createElement("textarea");
        document.body.appendChild(t);
        t.value = val;
        t.select();
        document.execCommand('copy');
        document.body.removeChild(t);
    },
    onClickGuarantee(num){
      var _target = this.$refs.guaranteeBox.childNodes[num].classList.contains('on');
      if(_target) this.currentTabMenu = num;
      // 보장 컨텐츠 클릭 시 애니메이션 초기화
      this.$refs.guaranteeBox.classList.toggle('on');
      setTimeout(() => this.$refs.guaranteeBox.classList.toggle('on'), 100);
    },
    onClickMovie(num){
      var _url = this.guaranteeArr[num].url;
      window.open(_url, "_blank");
    },
    onClickShare() {
      var _href = 'http://192.168.0.12:8080/#/',
          _text = '[내일을 바꾸는 건강생활 빙고] 빙고로 평소 생활습관을 체크하고 맞춤보장까지 알아보세요!' + encodeURIComponent("\n" + _href);
      location.href = 'sms:' + (this.checkMobile() == 'ios' ? '&' : '?') + 'body='+ _text;
    },
    checkMobile(){
      var varUA = navigator.userAgent.toLowerCase();
      if ( varUA.indexOf('android') > -1) {
        //안드로이드
        return "android";
      } else if ( varUA.indexOf("iphone") > -1 || varUA.indexOf("ipad") > -1 || varUA.indexOf("ipod") > -1 ) {
        //IOS
        return "ios";
      } else {
        //아이폰, 안드로이드 외
        return "other";
      }
    },
  },
};
</script>
<style>
.popupDimmed-enter,
.popupDimmed-leave-to {
  opacity: 0;
}
.popupDimmed-enter-active,
.popupDimmed-leave-active {
  transition: opacity 0.2s;
}
</style>