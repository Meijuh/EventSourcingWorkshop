# Workshop assignments

## Project structure

This projects holds a simple implementation of base classes required to implement event sourcing. These
base classes are located in the `digital.hd.workshop.eventsourcing.common.es` package.
A sample implementation of an Order aggregate is located in the `digital.hd.workshop.eventsourcing.domain.sales` package.
It holds an Order aggregate in `OrderAR.java`.
The main responsibility of the aggregate is to keep the aggregate consistent. 
It provides business methods to change the state of the aggregate. In order to check consistency aggregate keeps state.
The state of the aggregate is computed by replaying events on the aggregate every time a new event is applied.

Aggregates are stored and retrieved from a repository, see `EventSourcedRepository.java`.

A command handler is responsible for handling commands and applying events to aggregates. See `OrderCommandHandler.java`.

A view is a projection of the state of an aggregate. See `JDBCOrderView.java`. It subscribes to events and updates a
materialized view.

## Assignment 1: Getting started 

Run the application, inspect the h2 database and try the REST calls provided with the postman collection.

## Assignment 2: Replay the JDBCOrderView

The `JDBCOrderView` is a view that is built from the events that occur for the Order aggregate.
One of the great benefits of event sourcing is fault tolerance, which means that whenever something appears
to be wrong in either our database or in the logic that updates the view, we have to opportunity to recompute the
view.

Let's try this by corrupting our database table first and then replay the view.

Update the `ORDER_OVERVIEW` table and set the value of `ITEM_COUNT` for all rows to 100.

Call the `view/List orders (overview)` method using Postman. You should see that the item count is 100 for all orders.

Now let's replay the view. Call the `view/Replay order overview view` endpoint method using Postman.

If you now call the `view/List orders (overview)` method using Postman, you should see that the item count is correct again.

## Assignment 3: Extend the OrderItemAdded event

Extend the OrderItemAdded event with the price. 
The price should be a `BigDecimal` and should be stored in the event.

## Assignment 4: Update the JDBCOrderView

Now we've added the price to the order, we want to include the total price of the order in the view.
Update the `JDBCOrderView` logic that handles the OrderItemAdded event and include the total price of the order.
Please note the database creation script in `src/main/resources/schema.sql` that creates the `ORDER_OVERVIEW` table.

## Assignment 5: Add an OrderItemRemoved event.

Implement an additional event that removes an item from the order.
Obviously the command to remove an item from the order should only succeed if the item is actually in the order.

## Assignment 6: Update the JDBCOrderView for removed items

Update the `JDBCOrderView` logic that handles the OrderItemRemoved event and update the total price of the order.