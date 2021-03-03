<template>
  <div id="app">
    <p>Expenses ({{ expensesArray.length }})</p>
    <input
      v-on:keyup.enter="clickAdd"
      placeholder="Title"
      type="text"
      v-model="expense.title"
    />
    <input
      v-on:keyup.enter="clickAdd"
      placeholder="Amount"
      type="number"
      v-model="expense.amount"
    />
    <input type="datetime-local" v-model="expense.datetime" />
    <button @click="clickAdd">Add</button>
    <br />
    <br />

    <button @click="clickClear">Clear all</button>

    <br />
    <br />

    <table class="results" v-if="expensesArray.length">
      <tr>
        <th>Title</th>
        <th>Amount</th>
        <th>Date</th>
        <th>Actions</th>
      </tr>
      <tr class="hashtag" v-for="(hash, i) in expensesArray" :key="i">
        <td>{{ hash.title }}</td>
        <td>{{ hash.amount }}</td>
        <td>{{ hash.datetime }}</td>
        <td>
          <button @click="clickRemove(i)">Remove</button>
        </td>
      </tr>
    </table>
    <!-- <HelloWorld msg="Hello Vue in CodeSandbox!" />-->
  </div>
</template>

<script>
//import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",
  components: {
    //HelloWorld,
  },
  data() {
    return {
      expensesArray: [],
      expense: {
        title: "",
        amount: null,
        datetime: this.getDateTime(),
      },
      limit: 30,
      data: this.getDateTime(),
    };
  },
  mounted: {},
  created: function () {
    const t = this;

    console.log("created");
    t.getToLocalstorage();
  },
  methods: {
    clickAdd() {
      const t = this;
      //debugger;
      var obj = {
        title: t.expense.title.trim(),
        amount: t.expense.amount.trim(),
      };

      if (obj) {
        obj.datetime = t.expense.datetime;

        t.expensesArray.push(obj);

        //debugger;
        t.setToLocalstorage();

        t.expense = {
          title: "",
          amount: null,
          datetime: this.getDateTime(),
          //t.asfasf()
        };
      }
    },
    clickRemove(i) {
      const t = this;
      t.expensesArray.splice(i, 1);
      t.setToLocalstorage();
    },
    clickClear() {
      const t = this;
      t.expensesArray = [];
    },
    formatCurrency(numb) {
      return (
        "$ " + numb.toFixed(2).replace(/(\d)(?=(\d{3})+(?:\.\d+)?$)/g, "$1,")
      );
    },
    getDateTime() {
      var now = new Date();
      now.setMinutes(now.getMinutes() - now.getTimezoneOffset());
      return now.toISOString().slice(0, 16);
    },
    setToLocalstorage() {
      const t = this;
      var toLs = JSON.stringify(t.expensesArray);
      //debugger;
      localStorage.setItem("keydada", toLs);
      //localStorage.setItem("HJIKHJIKHJIK", toLs);
    },
    getToLocalstorage() {
      const t = this;
      var toLs = JSON.parse(localStorage.getItem("keydada"));
      t.expensesArray = toLs;
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  max-width: 100%;
  border: solid;
  padding: 15px;
}
.results {
  width: 100%;
  border: solid;
}
.results tr th {
  background: rgb(0 0 0 / 10%);
}
.results tr td {
  background: rgb(0 0 0 / 2%);
}
</style>
