# Order matching engine

```
		    +-------------------------+
            |       User Client       |
            +-------------------------+
                        |
                        | (HTTP request, buy/sell order) 
				        |
            +-----------+-----------+
            |      Phoenix API      |
            +-----------+-----------+
                        |
                        | (seperate endpoints for buy/sell order)
                        |
            +-----------+-----------+
            |        RabbitMQ       |
            +-----------+-----------+
                        |
                        | (seperate queues for buy/sell order)
                        |
    +-------------------+-------------------+
    | Order Matching Engine(Phoenix logic)  |
    +-------------------+-------------------+
                        |
                        | (interact with buy and sell queue using algorithm and make a match)
                        |
    +-------------------+-------------------+
    |               Database                |
    +-------------------+-------------------+
                        |
                        | (save results to db)
                        |
    +-------------------+-------------------+
    |          send back response           |
    +-------------------+-------------------+
```

    The diagram shows the main components of the system:

    User Client: Allows users to submit orders.

    Phoenix API: API layer processes incoming requests from the user client and sends messages to RabbitMQ.

    RabbitMQ: Message broker that handles communication between the Phoenix API and the order matching engine. Can handle a large number of concurrent connections and process messages asynchronously, ensuring that orders are matched quickly and accurately.

    Order Matching Engine: Core application logic that matches orders and executes trades. It receives messages from RabbitMQ and updates the database accordingly.

    Database: Persistent storage layer that stores orders.
