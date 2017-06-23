## Welcome to Our Hiring Exercise

Awesome!  Are you excited?  Pumped up?  Ready to go?  Great!

### What is this?

We want to be sure that we're on the same page as far as your ability and our expectations of you, and so have this exercise that we use for a few reasons:

- To get a sense for how you write code
- To get a sense for how you test code
- To get a sense for how we can communicate with you about your code

So we put together this exercise that usually takes on the order of 60-90 minutes to do.  Don't overthink it.

At a high level, you're going to create an accounting API in Rails (http://edgeguides.rubyonrails.org/api_app.html).  We don't use Rails all that much in our systems, but since it's the _Lingua Franca_ of Ruby, it's easy to work in.

### Requirements

- You should start a new project using `rails new #{github-username}-account-api-example --api --pg`
- Immediately mark an initial commit - this will serve as a starting point
- On completion, create a commit called "code-complete"
- Feel free to do refactoring/design improvements after that point in further commits
- This project will be `git clone`d by me, then `rake db:migrate && rake test && bin/rails s`.  If it needs more configuration than that, then it's probably not a great sign
- There should be good test coverage of key parts of the application

#### The application will have two components

1. Accounts - accounts can be created by POSTing to the `/accounts` endpoint, and shown by GETting `/accounts/id`
1. Deposits/Withdrawals - can be created by POSTing to `/accounts/:id/deposit` with a (json) body of `{amount: 1}` or `/accounts/:id/withdraw`.

####

1. The account should not be able to be overdrawn
1. An attempted overdraft should return an error status (422)

### Submitting

- Push this repository to github
- Email justin@obsidianexchange.com with the location of the repository - if you would like it to be private, then add the github user `litch` as a collaborator
- I will review the code within 24 hours and we can discuss it in github or Slack

### Criteria

- Clarity
- Adherence to standards
- Testing methodology/coverage
- "Does it work"
- Time to complete
- Below are some of the commands that I will use to validate the output of your application

### Create an account

```bash
% curl -X POST localhost:3000/accounts
{"id":4}
```

### Make a deposit

```bash
% curl -H "Content-Type: application/json" -X POST -d '{"amount":1}' localhost:3000/accounts/4/deposit

{}
```

### Make a withdrawal

```bash
% curl -H "Content-Type: application/json" -X POST -d '{"amount":1}' localhost:3000/accounts/4/withdraw

{}  
``` 

### Look at balance & transaction history

```bash
% curl localhost:3000/accounts/4
{"balance":0,"transactions":[{"id":7,"account_id":4,"amount":1,"created_at":"2017-06-23T15:02:21.336Z","updated_at":"2017-06-23T15:02:21.336Z"},{"id":8,"account_id":4,"amount":-1,"created_at":"2017-06-23T15:02:25.561Z","updated_at":"2017-06-23T15:02:25.561Z"}]}
```                              

### Prevents overdrafts

```bash
curl -H "Content-Type: application/json" -d '{"amount":100}' -w "%{http_code}" localhost:3000/accounts/4/withdraw

{}
422

% curl localhost:3000/accounts/4
{"balance":0,"transactions":[{"id":7,"account_id":4,"amount":1,"created_at":"2017-06-23T15:02:21.336Z","updated_at":"2017-06-23T15:02:21.336Z"},{"id":8,"account_id":4,"amount":-1,"created_at":"2017-06-23T15:02:25.561Z","updated_at":"2017-06-23T15:02:25.561Z"}]}                              
```
