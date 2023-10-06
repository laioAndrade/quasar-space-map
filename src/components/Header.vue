<template>
  <header>
    <img src="../assets/quasar_logo.png" alt="quasar logo" />
  </header>
</template>

<script>
export default {
  name: 'HeaderComponent'
}
</script>

<style scoped>
header {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  padding: 15px 40px;
  position: sticky;
}

header img {
  max-height: 70px;
}

@media (max-width: 420px) {
  header {
    justify-content: center;
  }
}
</style>
