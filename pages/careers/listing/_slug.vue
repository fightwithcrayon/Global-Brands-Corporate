<template>
  <main class="container single-post single-listing">
    <article>
      <i class="close custom-icon-cross h1" v-on:click="close"></i>
      <div class="post col-12 col-md-10 col-lg-8">
        <img src="~static/img/logo-colour.png" class="col-6 col-lg-4 watermark" />
        <h1 v-html="data.title.rendered"></h1>
        <p class="location h3">{{data.location}}</p>
        <p class="date">Added: {{postdate}}</p>
        <div class="content" v-html="data.content.rendered"></div>
        <ul class="footer">
          <li class="apply">
            <i class="icon-download"></i> <a :href="'mailto:info@globalbrands.co.uk?subject=' + subjectLine">Apply for this role</a>
          </li>
          <SocialLinks :story="data" />
        </ul>
      </div>
    </article>
  </main>
</template>

<script>
import axios from 'axios'
import {format, parseISO} from 'date-fns'
import SocialLinks from '~/components/SocialLinks.vue'

export default {
  head () {
    return {
      title: this.title,
      meta: [
        { hid: 'description', name: 'description', content: 'Leading independent drinks brand business Global Brands are looking for a new ' + this.data.title.rendered }
      ]
    }
  },
  components: {
    SocialLinks
  },
  computed: {
    postdate: function () {
      var newdate = parseISO(this.data.date)
      return format(newdate, 'Do MMMM yyyy')
    },
    subjectLine: function () {
      return encodeURI('Application for ' + this.data.title.rendered + ' role')
    }
  },
  methods: {
    close: function () {
      this.$router.go(-1)
    },
    isWide: function (index) {
      if (index === 0 || index === 6) {
        return true
      } else {
        return false
      }
    }
  },
  async asyncData (context) {
    let { data } = await axios.get(context.store.state.ajaxurl + '/wp/v2/jobs/?slug=' + context.params.slug)
    return {
      data: data[0],
      title: data[0].formatted_title
    }
  }
}
</script>
