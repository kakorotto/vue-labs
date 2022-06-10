<template>
  <div class="container" id="myapp">
    <div
      class="d-flex justify-content-between align-itmes-baseline bg-dark text-light mt-2 px-2"
    >
      <a href="#" class="text-warning" @click.prevent="islistvisible = false">
        book</a
      >
      <div class="d-flex align-items-baseline">
        <p>
          {{ booklist.length }}
          <span v-if="booklist.length > 1">Books </span>
          <span v-else>Book </span>
        </p>
        <span> | With Total Price: {{ handleCurrency(getTotalPrice()) }}</span>
        <button class="btn btn-primary mx-2" @click="islistvisible = true">
          Show List
        </button>
      </div>
    </div>
    <!-- end of the navbar -->
    <!-- books -->
    <div class="row justify-content-center mb-4" v-if="iscartvisible == false">
      <div
        v-for="book in books"
        :key="book.ISBN"
        class="card"
        style="width: 25rem; margin: 0.4rem"
      >
        <img
          :src="book.img + '&text=' + book.title"
          class="card-img-top"
          alt="..."
        />
        <div class="card-body">
          <h5 class="card-title">{{ book.title }}</h5>
          <p class="card-text">{{ book.subtitle }}</p>
        </div>
        <ul class="list-group">
          <ul
            class="list-group list-group-horizontal justify-content-center mt-2"
          >
            <li class="list-group-item">Category: {{ book.category }}</li>
            <li class="list-group-item">Author: {{ book.author }}</li>
          </ul>
          <ul
            class="list-group list-group-horizontal justify-content-center m-2"
          >
            <li
              :class="[
                book.pages >= 50 ? 'more' : '',
                book.pages < 50 ? 'less' : '',
              ]"
              class="list-group-item"
            >
              pages: {{ book.pages }}
            </li>
            <li class="list-group-item">Price: {{ book.price }}</li>
          </ul>
          <ul
            class="list-group list-group-horizontal justify-content-center mb-2"
          >
            <li class="list-group-item">ISBN: {{ book.ISBN }}</li>
            <li class="list-group-item">
              <button
                class="btn btn-secondary"
                :disabled="checkExist(book.ISBN)"
                @click="addToList(book)"
              >
                Add to list
              </button>
            </li>
          </ul>
        </ul>
        <div
          class="card-footer d-flex justify-content-between align-items-baseline"
        >
          <p
            :class="[
              book.pages >= 50 ? 'more' : '',
              book.pages < 50 ? 'less' : '',
              book.pages == 0 ? 'none' : '',
            ]"
          ></p>
        </div>
        <div class="card-body">
          <a href="#" class="card-link">Card link</a>
          <a href="#" class="card-link">Another link</a>
        </div>
      </div>
    </div>
  </div>
  <!-- end of the container -->
  <div class="row" v-if="islistvisible == true">
    <h4 class="text-center text-danger" v-if="booklist.length == 0">
      Sorry, cart is empty add books
    </h4>
  </div>
  <div class="watchList" v-if="islistvisible == true">
    <div class="table-responsive" v-if="booklist.length > 0">
      <table class="table table-hover table-bordered table-striped text-center">
        <thead>
          <tr>
            <th>Book Name</th>
            <th>Author</th>
            <th>Pages</th>
            <th>Price</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in booklist" :key="item">
            <td>{{ item.book.title }}</td>
            <td>{{ item.book.author }}</td>
            <td>{{ item.book.pages }}</td>
            <td>{{ handleCurrency(item.book.price) }}</td>
            <td>
              <button
                class="btn btn-danger"
                type="button"
                @click="removeFromList(item)"
              >
                -
              </button>
            </td>
          </tr>
        </tbody>
        <tfoot>
          <tr>
            <th colspan="3">Total Price:</th>
            <td colspan="3">{{ handleCurrency(getTotalPrice()) }}</td>
          </tr>
        </tfoot>
      </table>
    </div>
  </div>
</template>

<script>
import books from "../books.js";
export default {
  name: "BookApp",
  data: () => ({
    books: books,
    isbookListAailable: false,
    bookList: { items: [] },
  }),
  methods: {
    addToList(book) {
      this.bookList.items.push({ book });
      book.instock--;
    },
    checkExist(book) {
      return this.bookList.items.some((item) => item.book.ISBN == book.ISBN);
    },
    getTotalPrice() {
      let total = 0;
      for (let item of this.booklist) {
        total += item.book.price;
      }
      return total;
    },
    handleCurrency(value) {
      return new Intl.NumberFormat("en-US", {
        style: "currency",
        currency: "SAR",
      }).format(value);
    },
    removeFromList(book) {
      this.booklist.splice(
        this.booklist.findIndex((item) => item.book.ISBN == book.book.ISBN),
        1
      );
    },
    getTotalItems() {
      let total = 0;
      for (let i = 0; i < this.bookList.items.length; i++) {
        total +=
          this.bookList.items[i].quantity * this.bookList.items[i].book.price;
      }
      return total;
    },
  },
  computed: {},
  components: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card > p {
  text-align: justify;
}

.less {
  color: orange;
}

.more {
  color: green;
}
</style>
