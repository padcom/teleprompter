<template>
  <section class="page-template">
    <header class="header">
      <slot name="header">Header</slot>
    </header>
    <aside class="sidebar">
      <slot name="sidebar">Sidebar</slot>
    </aside>
    <main class="content">
      <slot>Content</slot>
    </main>
    <footer class="footer">
      <slot name="footer">Footer</slot>
    </footer>
  </section>
</template>

<style scoped>
.page-template {
  margin: -8px -20px 0px 0px;
  width: 100%;
  height: calc(100vh - 8px);
  display: grid;
  grid-template-areas: 
    ". header header ."
    ". sidebar content ."
    ". footer footer .";
  grid-template-columns: 1fr 100px 300px 1fr;
  grid-template-rows: 50px 1fr 30px;
}

.header {
  grid-area: header;
  background-color: #aec4db;
}

.sidebar {
  grid-area: sidebar;
  background-color: #f3ffdd;
}

.content {
  grid-area: content;
  background-color: white;
}

.footer {
  grid-area: footer;
  background-color: #ffdefb;
}
</style>