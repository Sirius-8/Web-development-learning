# What is The API?

it is a bridge that provides to talk the each other two software.

So, applications allow to each other for use their features and data.

> **Example:** Customer(_client_) can not cook the something in the restaurant. They only select to what does their want and says to waiter. Waiter(API) takes the order, goes to the kitchen and says to the chef(_database/server_).

## GET

read or take. It use about only read and take the information from the server/database.

Information moves in the JSON package.

## POST

send the data to server or create a new record.

## PUT

update or change data in the server.

## DELETE

delete data permanently.

## JSON(JavaScript Object Notation)

It is a message language in API's message.

It provides to computer and people read easily and store the data.

It is a text format.

## What is the Endpoint?

Spesifically web address(URL). It create when develop the backend.

Hospital: `https://api.hastane.com` (Base URL), endpoints: `/patients`, `/doctors`, `/appointments`...

## True Endpoint Structure

-`/products` -`/users`

## False Endpoint Structure

-`/get-products` -`/delete-user` -`/user` -`/product`

Name must be plural.
