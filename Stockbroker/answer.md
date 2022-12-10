# Question

- Design a system that support **market orders of stock** (only the core trading aspect of the platform)
- Design a system around a **place trade API call** from the user, and define the API call (what is input & response)
- Wont interact directly with bank because we have a SQL table for customers' balances
- Come up with a reasonable schema for that SQL
- How many customer? Scale to 1m customers and 1m trade/day and available only in U.S region
- High availability for services is needed
- Design UI ? No, should design what kind API that frontend will call to backend
- It will interacting with other exchange APIs because this system is the intermediate platform between customer and exchange platform.
- We will have a callback that can pass to exchange API. When the status of the exchange changes, we will get notification

<hr>

# Answer

**Plan of attacks** :

- API call
- API servers
- Trade execution system

<br>

**API call:**

- Call placeTrade(customerId, stockTicker, type, quantity)
- Response with (orderId, stockTiker, type, quantity, status, createdAt)

<br>

**API servers:**

**trade table**

| id  | c_id | s_t | type | quantity | status | created_at |
| :-- | :--- | :-- | :--- | :------- | :----- | :--------: |
|     |
|     |

<br>

**trade table**

| id  | c_id | amunt | last_m |
| :-- | :--- | :---- | :----- |
|     |
|     |

<br>

We obviously need a load balancer to distribute the request through out our network and we can use round robin as a load balancer strategy.
