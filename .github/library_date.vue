/* eslint-disable */
<template>
  <div class="backimg">
    <div style="display:block">
      <img src='./lef.png' class="lef-logo">
      <img src='./rig.png' class="rig-logo">
    </div>
    <div style="display:block">
      <div class="fon">
        대 출 일
      </div>
      <div class="box">
        <div class="boxfon" style="padding-top:10px">{{ nowDate }}</div>
        <br />
        <div class="boxfon">{{ nowday }}</div>
      </div>
    </div>
    <br />
    <div>
      <div class="fon">
        반 납 일
      </div>
      <div class="box">
        <div class="boxfon" style="padding-top:10px">{{ retDate }}</div>
        <br />
        <div class="boxfon">{{ retday }}</div>
      </div>
    </div>
    <div class="botfix">
      반납일을 지켜주세요!
    </div>
  </div>

</template>

<script>

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      timer: null,
      nowWeek: '',
      nowDate: '',
      nowTime: ''
    }
  },
  mounted() {
    this.timer = setInterval(() => {
      this.setNowTimes()
    },)
  },
  methods: {
    setNowTimes() {
      let rest = [{m:9,d:16},{m:9,d:17},{m:9,d:18},
      {m:10,d:3},{m:10,d:7},{m:10,d:9},{m:10,d:14},{m:11,d:4},{m:11,d:11},
      {m:12,d:2},{m:12,d:9},{m:12,d:25},
      {m:1,d:1},{m:1,d:6},{m:1,d:13},{m:1,d:28},{m:1,d:29},{m:1,d:30},
      {m:2,d:3},{m:2,d:10},{m:3,d:1},{m:3,d:3},{m:3,d:10}
    ]
      let myDate = new Date()
      let week = ['일', '월', '화', '수', '목', '금', '토'];
      let day = week[myDate.getDay()];
      let yy = myDate.getFullYear();
      let mm = myDate.getMonth() + 1
      let dd = myDate.getDate()
      this.nowDate = yy + '년 ' + day + '요일 '
      this.nowday = mm + '월' + dd + '일'
      let ret = new Date(myDate.setDate(myDate.getDate() + 14))
      let ry = ret.getFullYear();
      let rm = ret.getMonth() + 1
      let rd = ret.getDate()
      let rday = week[ret.getDay()];
      for(var i=0;i<rest.length;i++){
        if(rest[i].m==rm&&rest[i].d==rd){
          var tmp = new Date (ret)
          tmp.setDate(tmp.getDate()+1)
          ry = tmp.getFullYear();
          rm = tmp.getMonth() + 1
          rd = tmp.getDate()
          rday = week[tmp.getDay()];
          this.retDate = ry + '년 ' + rday + '요일 '
          this.retday = rm + '월' + rd + '일'
          ret=tmp;
        }
      }
      this.retDate = ry + '년 ' + rday + '요일 '
      this.retday = rm + '월' + rd + '일'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url('https://webfontworld.github.io/SCoreDream/SCoreDream.css');

@font-face {
  font-family: 'Cafe24Ssurround';
  src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_2105_2@1.0/Cafe24Ssurround.woff') format('woff');
  font-weight: normal;
  font-style: normal;
}

.backimg {
  background-image: url('backim.gif');
  height: 100vh;
  width: 100vw;
  margin: -8px;
  background-size: cover;
  background-repeat: no-repeat;
}

.fon {
  font-family: 'Cafe24Ssurround';
  float: left;
  font-size: 140px;
  font-weight: 500;
  color: white;
  margin-left: 20%;
  margin-top: 9rem;
  padding-top: 3rem;
}

.box {
  background-color: aliceblue;
  opacity: 0.8;
  display: inline-block;
  width: 25%;
  height: 10rem;
  margin-top: 10rem;
  margin-left: -10%;
  padding: 1rem;
}

.boxfon {
  font-family: 'Cafe24Ssurround';
  font-size: 60px;
  font-weight: 700;
  color: black;
}

.botfix {
  font-family: 'Cafe24Ssurround';
  font-size: 50px;
  position: fixed;
  bottom: 30px;
  color: white;
  margin-left: 35%;
}

.lef-logo {
  /* float: left; */
  left: 0;
  height: 6rem;
  margin-top: 15px;
  margin-left: -50%;
}

.rig-logo {
  float: right;
  right: 0;
  height: 4rem;
  margin-top: 15px;
  margin-right: 15px;
}
</style>