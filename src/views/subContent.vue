<template>
  <vsa-list>
    <vsa-item v-for='item in this.items' :key='item.id'>
      <vsa-heading>
        {{ item.name }}<span class="foundItem">1  festivals found.</span>
      </vsa-heading>

      <vsa-content>
        {{ item.recordLabel }}
      </vsa-content>
    </vsa-item>
  </vsa-list>
</template>

<script>
import VueSimpleAccordion from 'vue-simple-accordion'
import 'vue-simple-accordion/dist/vue-simple-accordion.css'
import Vue from 'vue'

Vue.use(VueSimpleAccordion, {})

export default {
  name: 'subContent',
  mounted () {
  },
  data () {
    return {
      listOfItems: [
        // data
      ]
    }
  },
  props: {
    items: Array
  }
}
</script>
