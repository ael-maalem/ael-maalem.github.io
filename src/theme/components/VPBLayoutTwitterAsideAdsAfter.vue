<template>
  <a
    class="twitter-timeline"
    :href="src"
    :data-theme="dark ? 'dark' : 'light'"
    :data-link-color="dataLinkColor"
    >Tweets by elmaalem_nacer</a
  >
</template>

<script>
import { nextTick } from 'vue'
export default {
  props: {
    src: {
      type: String,
      default: 'https://twitter.com/elmaalem_nacer?ref_src=twsrc%5Etfw',
    },
    dark: {
      type: Boolean,
      default: true,
    },
    dataLinkColor: {
      type: String,
      default: '#2B7BB9',
    },
  },
  async created() {
    let twitterFeed = document.createElement('script')
    // twitterFeed.setAttribute("src", "https://platform.twitter.com/widgets.js");
    const script = document.head.appendChild(twitterFeed)
    script.async = true
    await nextTick(() => {
      script.src = 'https://platform.twitter.com/widgets.js'
    })
  },
}
</script>
