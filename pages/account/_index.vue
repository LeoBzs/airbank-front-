<template>
  <div class="p-8">

    <div class="grid grid-cols-3 gap-8">
      <article class="flex flex-col items-start">
        <h2  class="font-bold font-mono text-green-400">
        {{ getTransactionById.account.username }}</h2>
        <div  class="text-sm text-gray-800">
          <p>Date: {{ getTransactionById.date }}</p>
          <p>Category: {{ getTransactionById.category }}</p>
          <p>Transaction ID: {{ getTransactionById.transactionId }}</p>
          <p>Account ID: {{ getTransactionById.account.accountId }}</p>
        </div>
      </article>
    </div>
    <NuxtLink
          :to="`/`"
          class="hover:bg-green-200 border rounded px-2 py-1 text-gray-800 border-gray-800 text-sm mt-2 font-bold font-mono">
          Back to transactions page
        </NuxtLink>
  </div>
</template>

<script>
import gql from 'graphql-tag'

const GET_TRANSACTION_BY_ID_QUERY = gql`
  mutation GetTransactionById($input: GetTransactionByIdInput) {
  getTransactionById(input: $input) {
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
      query: GET_TRANSACTION_BY_ID_QUERY,
      prefetch: true,
      variables: {
            input: this.params.accountId,
          },
    },
  },

/*
data() {
    return {
      searchText: '',
      searchResult: null,
      loading: false,
    }
  },
  methods: {
    async search() {
      try {
        const res = await this.$apollo.query({
          query: GET_TRANSACTION_BY_ID_QUERY,
          variables: {
            input: 1,
          },
        });
        if (res) {
          this.loading = false;
          const { result } = res.data.transactions;
          this.searchResult = result;
        }
      } catch (err) {
        this.loading = false;
        this.searchResult = [];
      }
    }
  }
  */
}

</script> 
