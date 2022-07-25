<template>
  <div class="p-8">
  <div class="container flex flex-wrap justify-between items-center mx-auto px-4 py-2">
    <h1 class="font-bold font-mono text-2xl mb-8">Transactions</h1>

    <form
      @submit.prevent="search"
      class="flex items-center mb-8"
    >
      <input
        type="text"
        name="Search"
        id="Search"
        class="border border-gray-600 rounded py-2 px-4 mr-2"
        v-model="searchText"
        placeholder="Search by ID"
      >
      <button
        type="submit"
        class="bg-gray-200 hover:bg-green-200 px-4 py-2 border border-gray-200 rounded"
      >
        Search
      </button>
    </form>
    </div>
    <div v-if="loading">Loading..</div>

    <div
      v-if="searchResults"
      class="mb-8"
    >
      <div v-if="searchResults.length">
        <p class="font-bold mb-2">Search results:</p>
        <div class="flex flex-wrap">
          <div
            v-for="getTransactionsByAccount in searchResults"
            :key="getTransactionsByAccount.transactionId"
            class="mr-4 mb-2 flex"
          >
            <NuxtLink
              :to="`/account/${getTransactionsByAccount.account.accountId}`"
              class="border rounded px-2 py-1 text-gray-800 border-gray-800 text-sm mt-2 whitespace-no-wrap"
            >
              {{ getTransactionsByAccount.category }}
            </NuxtLink>
          </div>
        </div>
      </div>
      <p
        class="font-bold font-mono"
        v-else
      >
        No results found
      </p>
    </div>

    <div class="grid grid-cols-3 gap-8">
      <article
        v-for="transaction in transactions"
        :key="transaction.id"
        class="flex flex-col items-start"
      >
        <h2 class="font-bold font-mono text-green-400">{{ transaction.account.username }}</h2>
        <div class="text-sm text-gray-800">
          <p>Date: {{ transaction.date }}</p>
          <p>Category: {{ transaction.category }}</p>
        </div>
        <NuxtLink
          :to="`/account/${transaction.account.accountId}`"
          class="hover:bg-green-200 border rounded px-2 py-1 text-gray-800 border-gray-800 text-sm mt-2 font-bold font-mono"
        >
          Read more
        </NuxtLink>
      </article>
    </div>
  </div>
</template>

<script>
import gql from 'graphql-tag'

const ALL_TRANSACTIONS_QUERY = gql`
  query ALL_TRANSACTIONS_QUERY {
  transactions {
    transactionId
    category
    date
    account {
      accountId
      username
    }
  }
}
`;

const TRANSACTIONS_BY_ID_QUERY = gql`
  mutation GetTransactionsByAccount($input: GetTransactionInput) {
  getTransactionsByAccount(input: $input) {
    transactionId
    category
    date
    account {
      username
    }
    accountId
  }
}
`;

export default {
  apollo: {
    transactions: {
      query: ALL_TRANSACTIONS_QUERY,
      prefetch: true,
    },
  },

  data() {
    return {
      searchText: '',
      searchResults: null,
      loading: false,
    }
  },

  methods: {
    async search() {
      try {
        const res = await this.$apollo.query({
          query: TRANSACTIONS_BY_ID_QUERY,
          variables: {
            input: this.searchText,
          },
        });

        if (res) {
          this.loading = false;
          const { results } = res.data.getTransactionsByAccount;
          this.searchResults = results;
        }
      } catch (err) {
        this.loading = false;
        this.searchResults = [];
      }
    }
  }
}
</script>