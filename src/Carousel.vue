<template>
  <div class="carousel">
    <div class="main">
      <div class="left-arrow" v-on:click="goToPrevious"></div>
      <div class="banner">
        <div
          v-bind:key="page.id"
          v-for="(page, index) in pages"
          class="content"
          v-bind:class="{ active: index === activePage }"
        >
          {{ page.text }}
        </div>
      </div>
      <div class="right-arrow" v-on:click="goToNext"></div>
    </div>
    <div class="footer">
      <div
        v-bind:key="page.id"
        v-for="(page, index) in pages"
        class="dot"
        v-bind:class="{ active: index === activePage }"
        v-on:click="goTo(index)"
      >
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data: () => ({
    activePage: 0,
    pages: [{ id: 1, text: '1' }, { id: 2, text: '2' }]
  }),
  methods: {
    goToPrevious(event) {
      this.activePage = Math.max(0, this.activePage - 1);
    },
    goToNext(event) {
      this.activePage = Math.min(this.pages.length - 1, this.activePage + 1);
    },
    goTo(page) {
      this.activePage = page;
    }
  }
};
</script>

<style scoped>
.carousel {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.main {
  display: flex;
  align-items: center;
}
.left-arrow {
  margin-right: 16px;
  border-top: 24px solid transparent;
  border-bottom: 24px solid transparent;
  border-left: 32px solid transparent;
  border-right: 32px solid blue;
}
.right-arrow {
  margin-left: 16px;
  border-top: 24px solid transparent;
  border-bottom: 24px solid transparent;
  border-left: 32px solid blue;
  border-right: 32px solid transparent;
}
.banner {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 300px;
  height: 100px;
  background: green;
}
.content {
  display: none;
  color: white;
  width: 90%;
  height: 90%;
  background: yellow;
  color: black;
}
.content.active {
  display: block;
}
.footer {
  display: flex;
  margin-top: 16px;
}
.dot {
  margin: 0 4px;
  border-radius: 50%;
  width: 16px;
  height: 16px;
  background: pink;
}
.dot.active {
  background: red;
}
</style>
