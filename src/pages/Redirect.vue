<template>
  <div class="hello">
    <h1>Você será redirecionado em instantes...</h1>
  </div>
</template>

<script>
export default {
  name: 'Redirect',
  created() {
    this.redirect();
  },
  methods: {
    redirect() {
      var desktopFallback = "https://youtube.com/watch?v=2RMGdUPNYz8",
      mobileFallback = "https://youtube.com/watch?v=2RMGdUPNYz8",
      app = "vnd.youtube://2RMGdUPNYz8";

      if( /Android|iPhone|iPad|iPod/i.test(navigator.userAgent) ) {
          window.location = app;
          window.setTimeout(function() {
              window.location = mobileFallback;
          }, 25);
      } else {
          window.location = desktopFallback;
      }

      function killPopup() {
          window.removeEventListener('pagehide', killPopup);
      }

      window.addEventListener('pagehide', killPopup);
    }
  }
}
</script>
