# Rolling update & roll backs


### Deployment Strategies
- "recreate strategy" : destroy all instances and spawn new ones ( has a down time)
- 'rolling update': doing one by one ( default one )


flow will be, update image on deployment and apply the deployment
- the it will crate a replica set , and starteedd deploying container & deleting the old ones from old replica sets
- to rollback , do `kubectl rollout undo deployment`



### Commands & arguments in definition files
### environment variables
- direct
- configMaps, requires evnfrom property
- secret, as secrets, as files  if volume ( only encoded )


### multi container pods
- side card, adapter & ambasidor
- init container
- agent for logs, metrics
- shares same network space & volumes