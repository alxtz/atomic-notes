- DynamoDB
   - **additional "virtual nodes" mapping are used**
      - had metadata (memory) overhead
- Jump Consistent Hash
   - better key distribution/less memory overhead
   - only shard number, (only) supports head/tail removal
- Loading Balancing
- more efficient cache usage when scaling
- libketama (a memcache client) [Damian Gryski「Consistent Hashing: Algorithmic Tradeoffs」](Consistent%20Hashing%20>%20參考資料.md)
- Cassandra(?) [Consistent Hashing > classic applications](Consistent%20Hashing%20>%20classic%20applications.md)