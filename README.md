# Airbank-Front

## Build 

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run start

# generate static project
$ npm run generate
```

# Details
This is my first time messing around with Vue3, i've had more experience with Angular and messed around with React beforehand, and i have a repo using it as a demo on my github as well.

  I found using it very intuitive, the details page has an error for the fact that the 1 in the URI isn't considered an int, even though it is of type graphqlInt, so maybe it could be attributed to the API. The other issue, the search, is a matter of rendering. i could make it fetch but not render, and found a lack of material around making it render on a query-formatted API. Either way, things that i could easily have fixed if i had found good reference material. 

  Besides that, i went for a very simple design there, using tailwind CSS. The search by catalog instead of by account name or date was just meant to test that functionality, the search using the account name would run on the same logic, and the one via date would simply use a destinated query for it, since id rather have the API doing the logic. 
