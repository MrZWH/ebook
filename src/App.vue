<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}

documnet.addEventListener('DOMContentLoaded', () => {
  const html = querySelector('html')
  let fontSize = window.innerWidth / 10;
  fontSize = fontSize > 50 ? 50 : fontSize;
  html.style.fontSize = fontSize + 'px'
})
</script>
