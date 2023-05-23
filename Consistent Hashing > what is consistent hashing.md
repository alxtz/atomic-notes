- conceptually
   - **classic "ring diagram" that maps "hashing space -> server id"**
     ![](0225005019.png)
     
---

- **is actually just an algorithmic variation of HashMap** [花花「Consistent Hashing」](Consistent%20Hashing%20>%20參考資料.md)
   - Consistent Hashing by itself, isn't actually the defining factor of your storage implementation
   - some implementations had data duplication across nodes, some doesn't (less conflicts to resolve)
   - data duplication in multi-nodes -> HA, had conflicts to resolve
   - not data duplication, just partitioning -> has single point of failure

---

- first seen
   - Consistent hashing and random trees: distributed caching protocols for relieving hot spots on the World Wide Web
   - Karger et al in the 1997 algorithm paper
   - **was originally designed for distributed caching (CDN)**
      - needs to support arbitrary node removal
      - when node fail, remove from cluster & reroute keys

---

- related applications
   - **it was called "consistent" as same request key are desired to be sent into the same bucket (server) even when total node changes**
   - values are expected to spread evenly (based on the random nature of hash, this isn't always true) across buckets (servers), even in node changes
      - "Consistent Hashing" by itself doesn't fix this, virtual nodes do