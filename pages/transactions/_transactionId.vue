<template>
  <div class="flex flex-col items-start p-4 space-y-4">
    <h1 class="font-bold text-2xl font-bold font-mono text-green-400">{{ transactionById.account.username }}</h1>
    <hr class="w-full">
    <div class="text-gray-800">
      <p>Category: {{ transactionById.category }}</p>
      <p>Date: {{ transactionById.date }}</p>
      <p>Id: {{ transactionById.account.accountId }}</p>
    </div>
    <NuxtLink
      to="/" class="hover:bg-green-200 border rounded px-2 py-1 text-gray-800 border-gray-800 text-sm">
      Go back
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
      accountId
      username
    }
  }
}
`;

export default {
  async asyncData({ app, params }) {
    const client = app.apolloProvider.defaultClient;
    const { transactionId } = params;

    //transactionId can equal 1, an int, so how come it throws a format error at the response? Using: GraphQLInt
    //if (transactionId == 1) { return console.log("1")}
    
    // to test the responses layout, just type an integer like 1 in place of the transactionId variable

    const res = await client.query({
      query: GET_TRANSACTION_BY_ID_QUERY,
      variables: { 
            input: { 
              transactionId: transactionId,
            },
          },
    })

    const { transactionById } = res.data;
    
    return {
      transactionById,
    }
  },
}
</script>