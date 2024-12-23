<template>
  <div>
    <NuxtPage />
  </div>
</template>

<script setup>
import { useHead } from '@unhead/vue'
import { onMounted } from 'vue'

useHead({
  script: [
    {
      hid: 'gtm-script',
      innerHTML: `(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
        new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
        j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
        'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
      })(window,document,'script','dataLayer','GTM-5C8HPJCZ');`,
      type: 'text/javascript',
      tagPriority: 1,
    },
  ],
  __dangerouslyDisableSanitizersByTagID: {
    'gtm-script': ['innerHTML'], // Avoid escaping the script content
  },
})

onMounted(() => {
  const noscript = document.createElement('noscript')
  noscript.innerHTML = `
    <iframe src="https://www.googletagmanager.com/ns.html?id=GTM-5C8HPJCZ"
    height="0" width="0" style="display:none;visibility:hidden"></iframe>
  `
  document.body.prepend(noscript)
})
</script>
