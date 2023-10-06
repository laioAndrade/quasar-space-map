<template>
  <footer>
    <span>Copyright © 2023 Quasar Space. All Rights Reserved.</span>
    <div class="icons">
      <a href="https://www.linkedin.com/company/quasarspace/" target="”_blank”"
        ><font-awesome-icon icon="fa-brands fa-linkedin" style="color: #ffffff" size="lg"
      /></a>
      <a href="https://www.facebook.com/quasarspacebr" target="”_blank”"
        ><font-awesome-icon icon="fa-brands fa-facebook" style="color: #ffffff" size="lg"
      /></a>
      <a href="https://www.instagram.com/quasarspacebr/" target="”_blank”"
        ><font-awesome-icon icon="fa-brands fa-instagram" style="color: #ffffff" size="lg"
      /></a>
    </div>
  </footer>
</template>

<script>
export default {
  name: 'HeaderComponent'
}
</script>

<style scoped>
footer {
  background: #232343;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 40px;
  color: #fff;
}

header img {
  max-height: 70px;
}

.icons {
  display: flex;
  gap: 20px;
  align-items: center;
}

@media (max-width: 420px) {
  footer {
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 20x 10px;
    text-align: center;
    gap: 16px;
  }
}
</style>
