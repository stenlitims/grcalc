<template>
  <div  id="app">
    <div class="container">
      <div class="calc">
        <div class="info">
          <div class="flat1">
            <div class="img">
              <img :src="flat.img_mini" alt="">
            </div>
            <div>
              <div class="type">Тип {{flat.type}}</div>
              <div class="dom">{{lang.dom}} {{flat.section}} / {{lang.floor}} {{flat.floor}}</div>
            </div>
          </div>
          <div class="flat2">
              <div class="c-item"> {{lang.sq}} - <span>{{flat.squaredb}}</span> м<sup>2</sup></div>
              <div class="c-item"> {{lang.price}} за м<sup>2</sup>  - <span>{{ number_format(price1Sq) }} грн.</span> </div>
              <div class="c-item"> {{lang.allPrice}}- <span>{{ number_format(summ) }}  грн.</span> </div>
          </div>
          <div class="f-last">
              <button class="btn btn-def" @click="openPop()">{{lang.chType}}</button>
          </div>
          
        </div>

        <div class="calc-intro">
            <div class="row">
              <div class="col-lg-6">
                  <div class="s-calc">
                    <div class="title">{{lang.firstVz}}</div>

                    <div class="procent"> <span>{{sl1.value}}</span>%</div>

                    <div class="pvz"> {{ number_format(pvz) }} грн.</div>

                    <div class="slider">
                        <vue-slider v-model="sl1.value" v-bind="sl1.options"></vue-slider>
                        <div class="min-max">
                          <div class="min">{{lang.min}}. {{sl1.options.min}}%</div>
                          <div class="max">макс. {{sl1.options.max}}%</div>
                        </div>
                    </div>


                    <div class="procent"> на <span>{{sl2.value}}</span> {{lang.mis}}.</div>


                    <div class="slider">
                        <vue-slider v-model="sl2.value" v-bind="sl2.options"></vue-slider>
                        <div class="min-max">
                          <div class="min">{{lang.min}}. {{sl2.options.min}} {{lang.mis}}.</div>
                          <div class="max">макс. {{sl2.options.max}} {{lang.mis}}.</div>
                        </div>
                    </div>

                     
                  </div>

              </div>
              <div class="col-lg-6">
                <div class="total-form">
                  <div class="total">
                    <div class="title">{{lang.mPl}}</div>
                    <div class="summ"> {{ number_format(pMonth) }} грн.</div>
                  </div>

                  <form action="" class="form">
                    <div class="form-group">
                      <input type="text" v-model="form.name" :placeholder="lang.name" class="form-control">
                    </div>
                    <div class="form-group">
                      <input type="text" id="phone" v-model="form.phone" placeholder="Телефон: +38(0__) ___-__-__" class="form-control">
                    </div>
                    <div v-if="fError" class="alert alert-danger text-center form-group" role="alert">
                      {{lang.fError}}
                    </div>
                    <div v-if="fSuccess" class="alert alert-success text-center form-group" role="alert">
                      {{lang.fSuccess}}
                    </div>

                    

                    <div class="text-center">
                      <button type="button" @click="send()" class="btn  btn-def">{{lang.zakRas}}</button>
                    </div>
                    

                    
                  </form>

                </div>

              </div>
            </div>
        </div>
      </div>




      <div class="calc-table" id="ctable">
        <div class="fill-row">
          <div class="fill-title">{{lang.rooms}}</div>
          <div class="rooms">
            <button class="btn" @click="setRoom(1)" :class="{ active: rooms == 1 }">1</button>
            <button class="btn" @click="setRoom(2)" :class="{ active: rooms == 2 }">2</button>
            <button class="btn" @click="setRoom(3)" :class="{ active: rooms == 3 }">3</button>
          </div>
        </div>

        <div class="row">
          <div class="col-md-6">
            <div class="fill-row">
              <div class="fill-title">{{lang.floor}}</div>
              <div class="slider">
                <div v-if="pop">
                  <vue-slider v-model="levels.value" v-bind="levels.options"></vue-slider>
                </div>
                  <div class="min-max">
                    <div class="min">{{lang.min}}. {{levels.options.min}}</div>
                    <div class="max">макс. {{levels.options.max}}</div>
                  </div>
              </div>
            </div>
          </div>
          <div class="col-md-6">
            <div class="fill-row">
              <div class="fill-title">{{lang.sq}}, м2</div>
              <div class="slider">
                <div v-if="pop">
                  <vue-slider v-model="sq.value" v-bind="sq.options"></vue-slider>
                </div>
                  <div class="min-max">
                    <div class="min">{{lang.min}}. {{sq.options.min}} м2</div>
                    <div class="max">макс. {{sq.options.max}} м2</div>
                  </div>
              </div>
            </div>
          </div>
        </div>

        

        <div v-if="data.length" class="wrap-table">
          <div v-if="isMobile" >
            <table class="table">
            <thead>
              <tr>
                <th scope="col">{{lang.oSq}}, м2</th>
                <th scope="col">{{lang.price}}, грн/м2</th>
                <th scope="col">{{lang.allPrice}}, грн</th>
                <th scope="col">Тип</th>
              </tr>
            </thead>
            <tbody>
            <tr class="flat-item" v-for="(item, i) in data" :key="i" @click="setFlat(item)" :class="{ active:isActive(item) }">
              <td>{{item.squaredb}}</td>
              <td>{{ number_format(item.price) }}</td>
              <td>{{ number_format(item.price *  item.squaredb ) }}</td>
              <td>{{ item.type }}</td>
            </tr>
            </tbody>
          </table>
          </div>
          <div v-else>
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">{{lang.oSq}}, м2</th>
                  <th scope="col">{{lang.floor}}</th>
                  <th scope="col">{{lang.price}}, грн/м2</th>
                  <th scope="col">{{lang.allPrice}}, грн</th>
                  <th scope="col">Тип</th>
                  <th scope="col">{{lang.dom}}</th>
                </tr>
              </thead>
              <tbody>
              <tr class="flat-item" v-for="(item, i) in data" :key="i" @click="setFlat(item)" :class="{ active:isActive(item) }">
                <td>{{item.squaredb}}</td>
                <td>{{item.floor}}</td>
                <td>{{ number_format(item.price) }}</td>
                <td>{{ number_format(item.price *  item.squaredb ) }}</td>
                <td>{{ item.type }}</td>
                <td>{{ item.section }}</td>
              </tr>
              </tbody>
            </table>
          </div>
        </div>

        <div v-else>
          <div class="text-center">
            <b>Пусто</b>
          </div>
        </div>
       

      </div>
    </div>
 
    
  </div>
</template>

<script>
import axios from "axios";
import Vue from "vue";
import VueLodash from "vue-lodash";
import Inputmask from "inputmask";

const options = { name: "lodash" };

Vue.use(VueLodash, options);
import vueSlider from "vue-slider-component";

let lang = {
  ru: {
    dom: "Дом",
    floor: "Этаж",
    sq: "Площадь",
    price: "Цена",
    allPrice: "Общая стоимость",
    chType: "Изменить тип",
    firstVz: "Первый взнос",
    min: "мин",
    mis: "мес",
    mPl: "Ежемесячный платеж на период рассрочки",
    name: "Ваше имя",
    zakRas: "Заказать рассрочку",
    rooms: "Комнат",
    oSq: "Общая площадь",
    fError: "Заполните все поля",
    fSuccess: "Спасибо. Мы с вами свяжемся."
  },
  uk: {
    dom: "Будинок",
    floor: "Поверх",
    sq: "Площа",
    price: "Ціна",
    allPrice: "Загальна вартість",
    chType: "Змінити тип",
    firstVz: "Перший внесок",
    min: "мін",
    mis: "міс",
    mPl: "Щомісячний платіж на період розстрочки",
    name: "Ваше ім'я",
    zakRas: "Замовити розстрочку",
    rooms: "Кімнат",
    oSq: "Загальна площа",
    fError: "Заповніть всі поля",
    fSuccess: "Дякуємо. Ми з вами зв'яжемося."
  }
};

let curLang = $("body").data("lang");
if(curLang == 'ua') curLang = 'uk';
let api = $("#api").data("api");
let idFlat = $("#api").data("id");

let isMobile = false;
if ($(window).width() < 768) {
  isMobile = true;
}

export default {
  name: "App",
  components: {
    vueSlider
  },
  data() {
    return {
      flats: [],
      flat: {},
      form: {
        name: "",
        phone: ""
      },
      isMobile: isMobile,
      idFlat: idFlat,
      lang: lang[curLang],
      month: 9,
      m30_49: 1500,
      m50_69: 1000,
      m70_99: 500,
      rooms: false,
      pop: false,
      fError: false,
      fSuccess: false,
      levels: {
        value: [1, 19],
        options: {
          width: "100%",
          height: 8,
          dotSize: 20,
          min: 1,
          max: 19
        }
      },
      sq: {
        value: [15, 90],
        options: {
          width: "100%",
          height: 8,
          dotSize: 20,
          min: 15,
          max: 90
        }
      },
      sl1: {
        value: 30,
        options: {
          width: "100%",
          height: 10,
          dotSize: 30,
          min: 30,
          max: 100,
          tooltip: false
        }
      },
      sl2: {
        value: 9,
        options: {
          width: "100%",
          height: 10,
          dotSize: 30,
          min: 1,
          max: 24,
          tooltip: false
          // disabled: true
        }
      }
    };
  },
  created() {
    axios
      .get(api)
      .then(response => {
        this.flats = Vue.lodash.sortBy(response.data, [
          function(o) {
            return o.squaredb;
          }
        ]);
        this.flat = Vue.lodash.find(response.data, { id: idFlat });

        if (!this.flat) {
          let vals = Object.values(response.data);
          this.flat = vals[0];
        }

        this.flat.price = +this.flat.price;
      })
      .catch(e => {
        this.erros.push(e);
      });
  },
  mounted() {
    var im = new Inputmask("+38(999)-99-99-999");
    im.mask(document.getElementById("phone"));
  },
  computed: {
    price1Sq() {
      let p = 0;
      if (this.sl2.value > 9 && this.sl1.value <= 99) {
        this.sl1.options.min = 50;
        p = 2000;
      } else {
        this.sl1.options.min = 30;
        if (this.sl1.value >= 30 && this.sl1.value <= 49) p = this.m30_49;
        if (this.sl1.value >= 50 && this.sl1.value <= 69) p = this.m50_69;
        if (this.sl1.value >= 70 && this.sl1.value <= 99) p = this.m70_99;
      }
      return this.flat.price + p;
    },
    summ() {
      return this.price1Sq * this.flat.squaredb;
    },
    pvz() {
      let data = this.summ / 100 * this.sl1.value;
      return data;
    },
    pMonth() {
      return (this.summ - this.pvz) / this.sl2.value;
    },
    data() {
      let data = this.flats;
      if (this.rooms) {
        data = Vue.lodash.filter(this.flats, { rooms: this.rooms });
      }
      data = Vue.lodash.filter(data, o => {
        return o.squaredb >= this.sq.value[0] && o.squaredb <= this.sq.value[1];
      });
      data = Vue.lodash.filter(data, o => {
        return (
          o.floor >= this.levels.value[0] && o.floor <= this.levels.value[1]
        );
      });
      return data;
    }
  },
  methods: {
    setRoom(room) {
      this.rooms = room;
    },
    number_format(number, decimals, dec_point, thousands_sep) {
      number = (number + "").replace(/[^0-9+\-Ee.]/g, "");
      var n = !isFinite(+number) ? 0 : +number,
        prec = !isFinite(+decimals) ? 0 : Math.abs(decimals),
        sep = typeof thousands_sep === "undefined" ? " " : thousands_sep,
        dec = typeof dec_point === "undefined" ? "." : dec_point,
        s = "",
        toFixedFix = function(n, prec) {
          var k = Math.pow(10, prec);
          return "" + (Math.round(n * k) / k).toFixed(prec);
        };
      s = (prec ? toFixedFix(n, prec) : "" + Math.round(n)).split(".");
      if (s[0].length > 3) {
        s[0] = s[0].replace(/\B(?=(?:\d{3})+(?!\d))/g, sep);
      }
      if ((s[1] || "").length < prec) {
        s[1] = s[1] || "";
        s[1] += new Array(prec - s[1].length + 1).join("0");
      }
      return s.join(dec);
    },
    isActive(menuItem) {
      // console.log(menuItem);
      return this.flat.id === menuItem.id;
    },
    setFlat(flat) {
      flat.price = +flat.price;
      this.flat = flat;
      $.fancybox.close();
      this.pop = false;
    },
    allPrice(flat) {
      let price = Math.ceil(flat.price * flat.squaredb);
      // this.flat.allprice = price;
      return this.number_format(price);
    },
    openPop() {
      $.fancybox.open({
        src: "#ctable",
        type: "inline",

        opts: {
          arrows: false,
          touch: false,
          animationDuration: 350,
          animationEffect: 'material',
          afterShow: (instance, current) => {
            this.pop = true;
          }
        }
      });
    },
    send() {
      let data = {};
      data.flat = this.flat;
      data.form = this.form;
      data.r = {
        price1Sq: this.number_format(this.price1Sq),
        summ:  this.number_format(this.summ),
        pvz:  this.number_format(this.pvz),
        pMonth:  this.number_format(this.pMonth),
        procent: this.sl1.value,
        month: this.sl2.value,
      }
      data.type = "calc";
      //console.log(data);

      if (this.form.name == "" || this.form.phone == "") {
        this.fError = true;

        setTimeout(() => {
          this.fError = false;
        }, 3000);

        return;
      }

      $('.total-form form').addClass('loader');

     
      $.post(
        "assets/plugins/requests.php",
        data,
        (response) => {
          console.log(response);
          if (response.type == "success") {
            this.fSuccess = true;
            this.form.name = '';
            this.form.phone = '';
             $('.total-form form').removeClass('loader');
             ga('send', 'event', 'calc_form', 'push');
            setTimeout(() => {
              this.fSuccess = false;
            }, 5000);
          }
        },
        "json"
      );
    }
  }
};
</script>

<style lang="scss">
.fill-row {
  margin-bottom: 30px;
  .slider {
    margin-top: 40px;
  }
}
.fill-title {
  margin-bottom: 7px;
}
.rooms {
  .btn {
    border-radius: 50%;
    width: 40px;
    height: 40px;
    margin: 0 5px;
    background: #ffd500;
    font-size: 14px;
    font-weight: bold;
    transition: all 0.3s ease;
    &:first-child {
      margin-left: 0;
    }
    &.active {
      background: #348f3c;
      color: #fff;
      &:focus {
        box-shadow: none;
      }
    }
  }
}
.btn-def {
  border-radius: 25px;
  background: #ffd500;
  padding: 12px 30px;
  font-size: 14px;
  font-weight: bold;
  text-transform: uppercase;
  color: #4d4d4d;
}
.btn:focus {
  outline: 0;
  box-shadow: 0 0 0 0.2rem rgba(255, 166, 0, 0.25);
}
.form-control:focus {
  border-color: rgba(255, 166, 0, 0.25);
  box-shadow: 0 0 0 0.2rem rgba(255, 166, 0, 0.25);
}

.calc-table {
  display: none;
}
.flat-item {
  cursor: pointer;
  &.active {
    background: rgba(255, 213, 0, 0.3);
  }
}
.calc {
  background: rgb(247, 247, 247);
  border-radius: 5px;
  border: 1px solid #f0f0f0;
  .info {
    display: flex;
    padding: 1em 1.5625em;
    align-items: center;
    border-bottom: 1px solid #f0f0f0;
    .img {
      background: #fff;
      padding: 7px;
      border: 1px solid #f0f0f0;
      border-radius: 3px;
      margin-right: 15px;
    }
    .flat1 {
      display: flex;
      align-items: center;
    }
    .flat2 {
      margin-left: 35px;
      margin-right: 35px;
      .c-item {
        color: #a0a0a0;
        span {
          color: #000;
          font-weight: bold;
        }
      }
    }
    .type {
      color: #348f3c;
      font-size: 26px;
      font-weight: bold;
    }
  }
}

.calc-intro {
  padding: 1.5em 1.5625em;
}

.s-calc {
  .title {
    text-align: center;
    font-size: 30px;
    font-weight: bold;
  }
  .slider {
    margin-bottom: 30px;
  }
}

.min-max {
  display: flex;
  justify-content: space-between;
  padding: 0 15px;
  margin-top: -7px;
}
.pvz {
  text-align: center;
  font-weight: bold;
  font-size: 20px;
}

.procent {
  text-align: center;
  font-size: 30px;
  font-weight: bold;
  span {
    font-size: 48px;
  }
}

body {
  .vue-slider-component .vue-slider-process {
    background-color: #ffd500;
  }
  .vue-slider-component .vue-slider-tooltip {
    background-color: #ffd500;
    border: 1px solid #ffd500;
    color: #000;
  }
}

.total-form {
  background: #fff;
  border-radius: 5px;
  border: 5px solid #ffd500;
  padding: 25px;
  position: relative;
  &:before,
  &:after {
    right: 100%;
    top: 3.125em;
    border: solid transparent;
    content: " ";
    height: 0;
    width: 0;
    position: absolute;
    pointer-events: none;
  }
  &:after {
    border-color: hsla(0, 0%, 84%, 0);
    border-right-color: #fff;
    border-width: 15px;
    margin-top: -15px;
  }
  &:before {
    border-right-color: #ffd500;
    border-width: 23px;
    margin-top: -23px;
  }
  .total {
    text-align: center;
    padding-bottom: 10px;
    border-bottom: 1px solid #f0f0f0;
    .title {
      font-weight: bold;
      font-size: 18px;
    }
    .summ {
      color: #348f3c;
      font-size: 40px;
      font-weight: bold;
    }
  }
  .form {
    padding-top: 40px;
    max-width: 360px;
    margin-left: auto;
    margin-right: auto;
    input {
      border-radius: 0;
      height: 46px;
      border-width: 2px;
    }
    .form-group {
      margin-bottom: 20px;
    }
  }
}
.calc-table {
  min-height: 100vh;
}

.fancybox-fx-material.fancybox-slide--previous,
.fancybox-fx-material.fancybox-slide--next {
    transform: translateY(-60px) scale(1.1);
    opacity: 0;
}

.fancybox-fx-material.fancybox-slide--current {
    opacity: 1;
    transform: translateY(0) scale(1);
}

.loader{
  position: relative;
  &:before{
    content: "" !important;
    position: absolute;
    left: 0;
    bottom: 0;
    right: 0;
    top: 0;
    background: rgba(255,255,255,.6);
    z-index: 50;
        width: 100%;
    height: 230px;
  }
}

@media (min-width: 991px) {
  .calc-table {
    min-width: 500px;
  }
}
@media (min-width: 1200px) {
  .calc-table {
    min-width: 800px;
  }
}
@media (max-width: 991px) {
  .total-form:before,
  .total-form:after {
    content: none;
    opacity: 0;
  }
  .s-calc .title {
    font-size: 27px;
  }
  .calc-table {
    .table {
      font-size: 13px;
    }
  }
  .table thead th {
  }
}

@media (max-width: 768px) {
  .calc .info {
    flex-wrap: wrap;
    justify-content: space-between;
  }
  .calc .info .flat2 {
    margin-top: 30px;
    margin-left: 0;
  }
}

@media (max-width: 520px) {
  .calc .info .flat2 {
    margin-bottom: 30px;
  }
  .f-last {
    width: 100%;
    text-align: center;
  }
  .s-calc .title {
    font-size: 24px;
  }
  .procent span {
    font-size: 38px;
  }
  .procent {
    font-size: 20px;
  }
  .total-form .total .summ {
    font-size: 34px;
  }
  .total-form {
    padding: 15px;
  }
  #ctable {
    padding: 30px;
    .table td, .table th{
      padding: .75rem 6px;
    }
    .wrap-table{
      margin-left: -15px;
      margin-right: -15px;
    }
  }
  .calc{
    margin-left: -20px;
    margin-right: -20px;
  }
}
@media (max-width: 400px) {
  .calc-table .table {
    font-size: 12px;
  }
}
</style>
