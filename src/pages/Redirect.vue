<template>  
  <teleport to="head">
    <meta lang="pt-BR">
    <meta name="title" :content="title">
    <meta name="description" :content="description">

    <meta property="og:locale" content="pt_BR">
    <meta property="og:type" content="article">
    <meta property="og:title" :content="title">
    <meta property="og:description" :content="description">
    <meta property="og:url" :content="desktopFallback">
    <meta property="og:site_name" content="YouTube">
    <meta property="og:image" :content="imagePreview">
    <meta property="og:image:url" :content="imagePreview">
    <meta property="og:image:width" content="470">
    <meta property="og:image:height" content="265">
    <meta property="og:video" content="https://www.youtube.com/v/B5SYqPLFqlg?version=3">
    <meta property="og:video:height" content="360">
    <meta property="og:video:type" content="application/x-shockwave-flash">
    <meta property="og:video:width" content="640">

    <meta itemprop="name" :content="title">
    <meta itemprop="description" :content="description">
    <meta itemprop="image" :content="imagePreview">

    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:title" :content="title">
    <meta name="twitter:description" :content="description">
		<meta name="twitter:site" content="@youtube">
		<meta name="twitter:image" :content="imagePreview">
  </teleport>
  <div class="hello">
    <h1>Você será redirecionado em instantes...</h1>
    <iframe :src="iframeUrl" allowfullscreen width="560" height="315" frameborder="0"
      allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" />
  </div>
</template>

<script>

const ytBrowserUrlTemplate = 'https://youtube.com/watch?v=[YT_VIDEO_KEY]'
const ytDeepLinkTemplate = 'youtube://[YT_VIDEO_KEY]'
const ytEmbedUrlTemplate = 'https://www.youtube.com/embed/[YT_VIDEO_KEY]'

export default {
  name: 'Redirect',
  data(){
    return {
      ytVideoKey: '2RMGdUPNYz8',
      title: 'Criando um aplicativo',
      description: 'Irei mostrar como criar um aplicativo do zero',
      imagePreview: 'https://yt2fb.com/images/preview/1644061685.jpg'
    }
  },
  created() {
    this.redirect(
      {
        desktopFallback: this.desktopFallback, 
        mobileFallback: this.mobileFallback, 
        deepLinking: this.deepLinking,
      }
    );
  },
  computed: {
    iframeUrl(){
      return ytEmbedUrlTemplate.replace('[YT_VIDEO_KEY]', this.ytVideoKey)
    },
    desktopFallback(){
      return ytBrowserUrlTemplate.replace('[YT_VIDEO_KEY]', this.ytVideoKey)
    },
    mobileFallback(){
      return ytBrowserUrlTemplate.replace('[YT_VIDEO_KEY]', this.ytVideoKey)
    },
    deepLinking(){
      return ytDeepLinkTemplate.replace('[YT_VIDEO_KEY]', this.ytVideoKey);
    }
  },
  methods: {
    redirect({desktopFallback, mobileFallback, deepLinking}) {
      if (/Android|iPhone|iPad|iPod/i.test(navigator.userAgent)) {
        window.location = deepLinking;
        window.setTimeout(function () {
          window.location = mobileFallback;
        }, 25);
      } else {
        window.location = desktopFallback;
      }

      function killPopup() {
        window.removeEventListener('pagehide', killPopup);
      }

      window.addEventListener('pagehide', killPopup);
    },
  },
};
</script>

<style lang="css" scoped>
iframe {
  max-width: 100%;
}
</style>
