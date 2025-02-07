---
tags:
  - database
---
> [!TL;DR]
It's just the use of Bloom filters in the join query section , 
instead of using [[nested loop join]] using a cache layer of bloom filter.
### Architecture


### Nested Loops Join
```python
orders: {id, data} = [(1, obj...), (7, ...), (21, ...)] # fact table  
customers: {order_id, data} = [(2, obj...), (7, ...), (14, ...)] # dimension table  
  
selected = []  
for order in orders:  
for customer in customers:  
# discards data when customer is 2 or 14  
if order.id == customer.order_id:  
selected.append(customer)
```

- with additional cache layer
```python
orders = [(1, obj...), (7, ...), (21, ...)]  
customers = [(2, obj...), (7, ...), (14, ...)]  
  
selected = []  
cache = {2: True, 7: True, 14: True}  
for order in orders:  
if order.id in cache:  
# we only check customers at cache hit  
for customer in customers:  
if order.id == customer.order_id:  
selected.append(customer)
```


### References

- On Release :  [3_38_0](https://www.sqlite.org/changes.html#version_3_38_0)
- Blog : https://avi.im/blag/2024/sqlite-past-present-future
- Paper : [SQLite: Past, Present, and Future](https://www.vldb.org/pvldb/vol15/p3535-gaffney.pdf)

