<template>

<div class="loader"></div>

</template>

<script>

export default {
  name: 'LoaderPage',
};

</script>

<style>

.loader {
  border: 10px solid transparent;
  border-top: 10px solid #FFFFFF;
  border-radius: 50%;
  width: 80px;
  height: 80px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

</style>
