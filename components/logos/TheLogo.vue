<template>
<div class="font-extrabold hover:text-hot-pink text-3xl tracking-tighter text-indigo-700">
  <img id="main-logo" src="/images/brain.svg" alt="">
</div>
</template>

<script>
export default {
}
</script>

<style scoped>
#main-logo {

  width: 65px;
}

.animation-logo {
  animation: bounce 1s infinite;
}

@keyframes bounce {
  0%, 100% {
    transform: translateY(-5%);
    animationTimingFunction: cubic-bezier(0.4, 0, 1, 1);
  }
  50% {
    transform: translateY(0);
    animationTimingFunction: cubic-bezier(0, 0, 0.2, 1);
  }
}

</style>
