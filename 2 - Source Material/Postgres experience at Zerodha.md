	A detailed note from zerodha , a stock broker platform. Enlisting about how they into postgress (from 8 to 13 as of 2024). This discussion includes how they are able to obtain finetuned postgress setup for their backend infra. with zero tolerance and no data loss so far!!


### Some notable use cases @zerodha
- Some systems are readonly throught day & write only during nights
- Stock market active @9:15 am to @3:00 pm
- Started as 100mb imports to 100gigs imports per day

> Any transaction done by the vendors & traders will be combined and only imported at nights 🌃, `Bulk wrights on night`

### Their way of managing big data
- Index but don't overdo it
- Denormalize data sets, ( incrase size though, but better and faster queries )
> Since with normalization, and given huge data bunch of join queries will take up lot's of resoruce.
- Postgres db tuning across queries, `Also note of tuning hi accessed tables`
- Having a single db instances, no replica & no master/slave. so for loadbalance using additional postgress as a cache layer with using as `sqljobber` [dungbeetle](https://github.com/zerodha/dungbeetle)
- Deleting lot of redundant data, i.e data that can be generated after certain set of computation.
- Rather than giving computing in app, let's postgres do it. `overload your postgress withy any computations, it can do prettymuch faster than your app`
## References
[Database Tuning at Zerodha - India's Largest Stock Broker](https://www.youtube.com/watch?v=XB2lF_Z9cbs)