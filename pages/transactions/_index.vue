<template>
  <div class="p-8">
    <div class="grid grid-cols-3 gap-8">
      <article class="flex flex-col items-start">
        <h2  class="font-bold font-mono text-green-400">
        {{ transactionById.account.username }}</h2>
        <div  class="text-sm text-gray-800">
          <p>Date: {{ transactionById.date }}</p>
          <p>Category: {{ transactionById.category }}</p>
          <p>Transaction ID: {{ transactionById.transactionId }}</p>
          <p>Account ID: {{ transactionById.account.accountId }}</p>
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
  query TransactionById($input: getTransactionByIdInput) {
  transactionById(input: $input) {
    transactionId
    category
    date
    account {
      username
    }
  }
}
`;

export default {
  apollo: {
    transactionById: {
      query: GET_TRANSACTION_BY_ID_QUERY,
      prefetch: true,
      variables: {
            input: { 
              transactionId: 2, // this.$route.params._index 
            },
          },
    },
  },
}

</script> 
