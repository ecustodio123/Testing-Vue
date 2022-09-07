<template>
  <div class="pt-75 w-50 mx-auto">
    <div class="layout-row align-items-center justify-content-center">
      <input
        type="date"
        class="large outlined"
        data-testid="app-input"
        placeholder="Filter Date"
        v-model="dateFilter"
      />
      <button class="" data-testid="submit-button" @click="filtrar(dateFilter)">
        Filter
      </button>
    </div>
    <div class="card mx-auto table-card mt-50 pb-10">
      <table>
        <thead>
          <tr>
            <th>Date</th>
            <th>Description</th>
            <th>Type</th>
            <th data-testid="amount" class="sortable" @click="orderAmount">
              Amount ($)
            </th>
            <th @click="orderBalance">Available Balance</th>
          </tr>
        </thead>

        <tbody data-testid="records-body">
          <tr v-for="(txn, index) in transactionsReceived" :key="index">
            <!-- Use this section to render the transactions -->
            <td>{{ txn.date }}</td>
            <td>{{ txn.description }}</td>
            <td>{{ txn.type === 1 ? "Debit" : "Credit" }}</td>
            <td>{{ txn.amount }}</td>
            <td>{{ txn.balance }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "RecordsTable",
  props: {
    transactions: Array,
  },
  data() {
    return {
      dateFilter: null,
      transactionsReceived: [],
      SortBoolAmount: true,
      SortBoolBalance: true,
    };
  },
  created() {
    this.transactionsReceived = this.transactions;
  },
  methods: {
    filtrar(dateFilter) {
      if (dateFilter !== null) {
        const data = this.transactionsReceived;
        let newTransactionsReceived = [];
        data.map((el) => {
          if (el.date === dateFilter) {
            this.transactionsReceived = el;
            newTransactionsReceived.push(el);
            this.transactionsReceived = newTransactionsReceived;
          }
        });
      }
    },
    orderAmount() {
      if (this.SortBoolAmount) {
        this.transactionsReceived.sort(this.SortArrayAsc);
        this.SortBoolAmount = false;
      } else {
        this.SortBoolAmount = true;
        this.transactionsReceived.sort(this.SortArrayDesc);
      }
    },
    orderBalance() {
      if (this.SortBoolBalance) {
        this.transactionsReceived.sort(this.SortArrayAscBalance);
        this.SortBoolBalance = false;
      } else {
        this.SortBoolBalance = true;
        this.transactionsReceived.sort(this.SortArrayDescBalance);
      }
    },
    SortArrayAsc(x, y) {
      if (x.amount < y.amount) {
        return 1;
      }
      if (x.amount > y.amount) {
        return -1;
      }
      return 0;
    },
    SortArrayDesc(x, y) {
      if (x.amount > y.amount) {
        return 1;
      }
      if (x.amount < y.amount) {
        return -1;
      }
      return 0;
    },
    SortArrayAscBalance(x, y) {
      if (x.balance < y.balance) {
        return 1;
      }
      if (x.balance > y.balance) {
        return -1;
      }
      return 0;
    },
    SortArrayDescBalance(x, y) {
      if (x.balance > y.balance) {
        return 1;
      }
      if (x.balance < y.balance) {
        return -1;
      }
      return 0;
    },
  },
};
</script>

<style scoped>
</style>