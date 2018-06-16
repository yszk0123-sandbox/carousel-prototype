<template>
  <div class="carousel">
    <div class="main">
      <div class="left-arrow" v-on:click="goToPrevious"></div>
      <div class="banner">
        <div
          class="slide-container"
          v-bind:class="{ sliding, reverse }"
        >
          <div
            class="slide-content"
            v-bind:key="keyForPage(page, index)"
            v-bind:style="{ background: page.color }"
            v-for="(page, index) in slidingPages"
          >
            {{ page.text }}
          </div>
        </div>
      </div>
      <div class="right-arrow" v-on:click="goToNext"></div>
    </div>
    <div class="footer">
      <div
        class="dot"
        v-bind:class="{ active: index === currentPage }"
        v-bind:key="page.id"
        v-for="(page, index) in pages"
        v-on:click="goTo(index)"
      >
      </div>
    </div>
  </div>
</template>

<script>
const SLIDE_TIMEOUT = 500;
const DUMMY_PAGE = { dummy: true, id: 0, text: '3', color: 'blue' };

export default {
  data: () => ({
    sliding: false,
    currentPage: 0,
    previousPage: 1,
    reverse: false,
    pages: [
      { id: 1, text: '1', color: 'red' },
      { id: 2, text: '2', color: 'yellow' },
      { id: 3, text: '3', color: 'blue' }
    ]
  }),
  methods: {
    goToPrevious(event) {
      const length = this.pages.length;
      const to = (length + this.currentPage - 1) % length;
      this.slide(this.currentPage, to, true);
    },
    goToNext(event) {
      const length = this.pages.length;
      const to = (this.currentPage + 1) % length;
      this.slide(this.currentPage, to, false);
    },
    goTo(page) {
      const reverse = page < this.currentPage;
      this.slide(this.currentPage, page, reverse);
    },
    slide(from, to, reverse) {
      if (from === to) {
        return;
      }

      this.reverse = reverse;
      this.previousPage = from;
      this.currentPage = to;

      this.sliding = true;
      setTimeout(() => {
        this.sliding = false;
      }, SLIDE_TIMEOUT);
    },
    keyForPage(page, index) {
      return page.dummy ? 'dummy' + index : page.id;
    }
  },
  computed: {
    slidingPages() {
      if (!this.sliding) {
        return [DUMMY_PAGE, this.pages[this.currentPage], DUMMY_PAGE];
      }
      return this.reverse
        ? [
            this.pages[this.currentPage],
            this.pages[this.previousPage],
            DUMMY_PAGE
          ]
        : [
            DUMMY_PAGE,
            this.pages[this.previousPage],
            this.pages[this.currentPage]
          ];
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
  border-left: none;
  border-right: 32px solid blue;
}

.right-arrow {
  margin-left: 16px;
  border-top: 24px solid transparent;
  border-bottom: 24px solid transparent;
  border-left: 32px solid blue;
  border-right: none;
}

.banner {
  width: 300px;
  height: 100px;
  background: green;
  white-space: nowrap;
  overflow: hidden;
}

.slide-container {
  pointer-events: none;
  margin-left: -300px;
}
.slide-container:not(.reverse).sliding {
  transition: all 0.5s;
  transform: translateX(-300px);
}
.slide-container.reverse.sliding {
  transition: all 0.5s;
  transform: translateX(300px);
}

.slide-content {
  display: inline-block;
  color: white;
  width: 300px;
  height: 100px;
  border: 1px solid black;
  background: yellow;
  color: black;
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
  transition: opacity 0.3s;
  transition-delay: 0.2s;
}
.dot.active {
  opacity: 1;
}
</style>
