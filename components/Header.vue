<template>
  <header class="main-header">
    <div class="container">
      <div class="main-header__inner">
        <a href="#" class="main-header__logo"
          >SEXY <br />
          FORECAST</a
        >
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: 'Header',
}
</script>
<style lang="scss" scoped>
.main-header {
  padding: 15px 0;
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(1px);
  width: 100%;
  box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.01);
}
.main-header__logo {
  color: white;
  text-decoration: none;
  font-size: 24px;
  font-weight: 700;
}
</style>
