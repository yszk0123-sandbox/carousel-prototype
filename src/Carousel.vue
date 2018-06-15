<template>
  <div class="carousel">
    <div class="main">
      <div class="left-arrow" v-on:click="goToPrevious"></div>
      <div class="banner" v-bind:class="{ reverse: reverse }">
        <transition name="slide">
          <div
            v-bind:key="page.id"
            v-for="(page, index) in pages"
            v-if="index === activePage"
            class="content"
            v-bind:style="{ background: page.color }"
          >
            {{ pages[activePage].text }}
          </div>
        </transition>
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
    reverse: false,
    pages: [
      { id: 1, text: '1', color: 'red' },
      { id: 2, text: '2', color: 'yellow' },
      { id: 3, text: '3', color: 'blue' }
    ]
  }),
  methods: {
    goToPrevious(event) {
      this.reverse = true;
      const length = this.pages.length;
      this.activePage = (length + this.activePage - 1) % length;
    },
    goToNext(event) {
      this.reverse = false;
      const length = this.pages.length;
      this.activePage = (this.activePage + 1) % length;
    },
    goTo(page) {
      this.reverse = page < this.activePage;
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
  width: 300px;
  height: 100px;
  background: green;
  white-space: nowrap;
  overflow: hidden;
}

.content {
  display: inline-block;
  color: white;
  width: 300px;
  height: 100px;
  border: 1px solid black;
  background: yellow;
  color: black;
}

.content.slide-enter-active,
.content.slide-leave-active {
  transition: all 0.5s;
}

:not(.reverse) > .content.slide-enter,
:not(.reverse) > .content.slide-leave {
  transform: translateX(0);
}
:not(.reverse) > .content.slide-enter-to,
:not(.reverse) > .content.slide-leave-to {
  transform: translateX(-100%);
}

.reverse > .content.slide-enter,
.reverse > .content.slide-leave {
  transform: translateX(-100%);
}
.reverse > .content.slide-enter-to,
.reverse > .content.slide-leave-to {
  transform: translateX(0);
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
  background: red;
  opacity: 0.5;
  transition: opacity 0.5s;
}

.dot.active {
  opacity: 1;
}
</style>
