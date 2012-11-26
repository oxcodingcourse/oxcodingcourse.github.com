# Setting up the 'MCR Bar' rails app

## Creating the app

    rails new mcr_bar
    cd mcr_bar
    git init
    git add .
    git commmit -m "Initial commit"

## Starting the server

    rails s

If you now visit http://localhost:3000 you should see the standard rails welcome page.

## Creating the item model

The item model will hold the details of everything that can be purchased at the bar - primarily different types of drink. We may want to expand this model in future (e.g. by allowing specification of constituent parts), but for the time being we'll keep it simple and just specify `name`, `price` and `description`.

    rails g scaffold item name:string price:integer description:text

You'll see that we've chosen an `integer` type for price: we've chosen to store the price in pennies, which should circumvent the problems caused by storing fixed precision decimals as floating point numbers (which treat 3.49999999999... and 3.50 as the same numbers).
