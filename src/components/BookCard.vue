<template>
  <div class="card" v-bind:class="{ read: book.read }">
    <h2 class="book-title">{{ book.title }}</h2>
    <img v-if="book.isbn" v-bind:src="'http://covers.openlibrary.org/b/isbn/' + book.isbn + '-M.jpg'" />
    <h3 class="book-author">{{ book.author }}</h3>
    <div class="button-container" v-if="! enableAdd">
        <button class="mark-read" v-on:click.prevent="setRead(true)" v-if=" ! book.read">Mark Read</button>
        <button class="mark-unread" v-on:click.prevent="setRead(false)" v-if="book.read">Mark Unread</button>
    </div>
    <button v-if="enableAdd" v-on:click.prevent="addToReadingList(book)">Add to Reading List</button>
  </div>
</template>

<script>
export default {
    name: 'book-card',
    props: {
        book: Object,
        enableAdd: {
            type: Boolean,
            default: false
        }
    },
    methods: {
        setRead(value) {
            this.$store.commit('SET_READ_STATUS', {book: this.book, value: value});
        },
        addToReadingList(book) {
            let addedBook = Object.assign({ read: false }, book);
            delete addedBook.bestSeller;
            delete addedBook.newRelease;
            this.$store.commit('SAVE_BOOK', addedBook);
        }
    }
}
</script>

<style>
.card {
    border: 1px solid black;
    border-radius: 10px;
    width: 250px;
    height: 485px;
    margin: 20px;
    background-color: rgba(236, 236, 236, 0.554);
}

.card.read {
    background-color: rgba(211, 211, 211, 0.77);
}

.card .book-title {
    font-size: 1.4rem;
}

.card .book-author {
    font-size: 1rem;
}
</style>