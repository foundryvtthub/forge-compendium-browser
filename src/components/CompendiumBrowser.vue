<template>
  <div class="forge-compendium-browser">
    <compendium-library
      v-if="!showBook"
      :library="library"
      @select="selectBook"
    ></compendium-library>
    <compendium-book
      v-else
      ref="compendiumBook"
      :book="book"
      @exit="selectBook"
      @link="openLink"
    ></compendium-book>
  </div>
</template>

<script>
import CompendiumLibrary from "./CompendiumLibrary.vue";
import CompendiumBook from "./CompendiumBook.vue";

export default {
  name: "CompendiumBrowser",
  components: {
    CompendiumLibrary,
    CompendiumBook,
  },
  data: () => ({
    library: null,
    book: null,
  }),
  methods: {
    selectBook(id) {
      this.book = this.library.find((b) => b.id == id);
      if (id) {
        game.user.setFlag("forge-compendium-browser", "last-book", id);
      } else {
        game.user.unsetFlag("forge-compendium-browser", "last-book");
      }
    },
    openLink(bookId, packId, id) {
      const book = this.library.find((b) => b.id == bookId);
      if (book) {
        this.book = book;
        this.$refs.compendiumBook.openLink(packId, id);
      }
    },
  },
  computed: {
    showBook() {
      return this.book != null;
    },
  },
  mounted() {
    this.library = game.ForgeCompendiumBrowser.books;
    const lastBook = game.user.getFlag("forge-compendium-browser", "last-book");
    if (lastBook) {
      this.selectBook(lastBook);
    }
  },
};
</script>

<style>
.forge-compendium-browser {
  max-height: 100%;
  overflow: hidden;
}
.forge-compendium-browser .flexcontain {
  flex-grow: 0;
}
.forge-compendium-browser .compendium-muted {
  opacity: 0.4;
  font-weight: bold;
}
</style>