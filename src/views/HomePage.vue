<template>
  <div class='wrapper'>
    <div class='header'>
      <p>
      App Title
      <a style='cursor:pointer;position:relative;left:40%' @click='reload()'><img src='../assets/arrow.svg' style='width:17px;'></a>
      </p>
    </div>
    <article class='loading' v-show='loadingShow'>Loading Please wait...</article>
    <vsa-list v-if='listOfItems.length' style='margin-top:100px !important' class='firstList'>
      <vsa-item v-for='item in listOfItems' :key='item.id'>
        <vsa-heading>
          {{ item.name }}<span v-if='item.bands.length' class='foundItem'>{{item.bands.length+'  festivals found.'}}</span>
        </vsa-heading>

        <vsa-content>
          <Subcontent
            :items='item.bands'
          />
        </vsa-content>
      </vsa-item>
    </vsa-list>
  </div>
</template>

<script>
import VueSimpleAccordion from 'vue-simple-accordion'
import 'vue-simple-accordion/dist/vue-simple-accordion.css'
import Vue from 'vue'
import axios from 'axios'
import Subcontent from './subContent'

Vue.use(VueSimpleAccordion, {})

export default {
  name: 'HomePage',
  mounted () {
    this.loadInfo()
    // this.listOfItems = localStorage.getItem('info', JSON.stringify(data))
  },
  beforeCreate () {
    // localStorage.setItem('info', JSON.stringify(data))
  },

  data () {
    return {
      listOfItems: [],
      subItems: [],
      options: ['Local', 'Auto'],
      loadingShow: false
    }
  },
  methods: {
    loadInfo () {
      this.loadingShow = true
      var self = this
      axios
        .get('http://localhost:3000/')
        .then((response) => {
          if (response.status === 200) self.loadingShow = false
          let info = response.data
          localStorage.setItem('info', JSON.stringify(info))
          this.listOfItems = JSON.parse(localStorage.getItem('info'))
        })
        .catch((error) => {
          if (!error.response) {
            this.errorStatus = 'Error: Network Error'
          } else {
            this.errorStatus = error.response.data.message
          }
        })
    },
    reload () {
      this.loadInfo()
    }
  },
  components: {Subcontent}
}
</script>

<style>
dl, dt, div {
  border: none !important;
}
.vsa-list {
  --vsa-max-width: 720px;
  --vsa-min-width: 300px;
  --vsa-heading-padding: 1rem 1rem;
  --vsa-text-color: #373737;
  --vsa-border-color: rgba(0,0,0,0.2);
  --vsa-border-width: 1px;
  --vsa-border-style: solid;
  --vsa-border: var(--vsa-border-width) var(--vsa-border-style) var(--vsa-border-color);
  --vsa-content-padding: 1rem 1rem;
  --vsa-default-icon-size: 1;
  --vsa-color: rgba(0,0,0,.67);
  display: block;
  max-width: var(--vsa-max-width);
  min-width: var(--vsa-min-width);
  width: 100%;
  padding: 0;
  margin: 0;
  list-style: none;
  color: var(--vsa-text-color);
  margin: auto !important;
}
dl:hover, dt:hover {
  color: rgba(0,0,0,.67) !important;
}
.vsa-item__trigger:focus, .vsa-item__trigger:hover {
  background-color: white;
  color: rgba(0,0,0,.67);
}

button {
  padding: 0px !important;
}
.vsa-item__trigger__icon--is-default {
  width: 8px;
}
.vsa-item__trigger__icon--is-default:after, .vsa-item__trigger__icon--is-default:before {
  width: 13.8px;
}
.wrapper {
  width: 100%;
}
.vsa-item {
  box-shadow: 0 2px 1px 0px rgb(0 0 0 / 1%), 0 1px 1px 0 rgb(0 0 0 / 5%), 0 1px 3px 0 rgb(0 0 0 / 12%);
  transition: box-shadow 280ms cubic-bezier(.4,0,.2,1);
  display: block;
  position: relative;
  padding: 10px 20px ;
  border-radius: 4px;
  cursor: pointer;
  margin-bottom: 1rem;
}
.vsa-item__heading, button {
  cursor: pointer;
}
span {
  font-size: 1rem !important;
}
.header {
  position: fixed;
  height: 60px;
  background-color: green;
  margin-bottom: 20px;
  text-align: center;
  line-height: 60px;
  color: white;
  width: 100%;
  z-index: 10050;
}
.header p {
  position: relative;
  right: 20%;
  margin-top: 0;
}

.vsa-item__trigger__icon, .vsa-item__trigger__icon--is-default, .vsa-item__trigger__icon--is-active {
  transform: rotate(0deg);
  font-family: Roboto,'Helvetica Neue',sans-serif;
  font-size: 15px;
  font-weight: 400;
}
.vsa-item__trigger__icon::after, .vsa-item__trigger__icon--is-default::after, .vsa-item__trigger__icon--is-active::after {
  width: 5px;
  height: 5px;
  border-top: 1px solid transparent;
  border-left: 1px solid transparent;
  border-bottom:1px solid rgba(0,0,0,.67);
  border-right:1px solid rgba(0,0,0,.67);
  transform: rotate(45deg);
}

.vsa-item__trigger__icon--is-default::before {
  left: 0;
  -webkit-transform: rotate(180deg) translate3d(8px,22px,0);
  transform: rotate(45deg) translate3d(8px,8px,0);
  -webkit-transform-origin: 100%;
  transform-origin: 100%;
  background-color: white;
  width: 0;
}

.vsa-item__trigger__icon::after:hover, .vsa-item__trigger__icon--is-default::after:hover, .vsa-item__trigger__icon--is-active::after:hover {
  background-color: white !important;
}
.vsa-item__trigger__icon::after, .vsa-item__trigger__icon--is-default::after, .vsa-item__trigger__icon--is-active::after {
  background-color: white;
}
.foundItem {
  position: absolute;
  right: 10%;
  color: rgba(0,0,0,0.2);
  font-weight: 100;
}
.vsa-item__trigger__content {
  min-width: 100px;
  min-height: 22px;
}
.loading {
  max-width: 720px;
  min-width: 300px;
  height: 50px;
  background-color: rgba(0,128,0,0.9);
  margin: auto;
  position: relative;
  top: 300px;
  line-height: 50px;
  z-index: 10050;
  cursor: pointer;
  color: white;
  border-radius: 5px;
}
@media only screen and (max-width: 872px) {
  .firstList {
    width: 80% !important;
  }
}
.vsa-item__trigger[aria-expanded=true] .vsa-item__trigger__icon--is-default:after {
  -webkit-transform: rotate(-135deg) translate3d(-8px,8px,0) !important;
  transform: rotate(-135deg) translate3d(-8px,8px,0) !important;
}
.vsa-item__heading:hover,.vsa-item__trigger:hover {
  background-color: rgba(0,0,0,.05);
  border-radius: 5px;
}
.vsa-item__trigger:hover {
  padding: 0 10px !important;
}
.vsa-item__trigger:hover .vsa-item__trigger__icon.vsa-item__trigger__icon--is-default::after {
  background-color: transparent;
}
</style>
