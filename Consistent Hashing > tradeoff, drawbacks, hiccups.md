- commonly been shout out in system design interviews
   - [呂行「Consistent Hashing and why it might not be the correct answer to your system design interview」](Consistent%20Hashing%20>%20參考資料.md)

---

- doesn't work that well with traditional OLTP/relational DB partitioning needs
   - **in most database scenes, ppl don't usually "re-shard" the entire cluster if one node is temporarily down**
      - [呂行「Consistent Hashing and why it might not be the correct answer to your system design interview」](Consistent%20Hashing%20>%20參考資料.md)
      - as cost for data movement -> high
      - replicas for a single shard are used
      - **re-sharding only happens when planned capacity changes, not for regular reliability**

---

- requires knowledge about "total amount of nodes"
   - [question] can Bitcoin/other p2p networks omit this constraint?

---

- hashing by itself is already "deterministic", calling it "Consistent Hashing" doesn't describe its intend well

---

- why is this still a research topic
   - uneven loading distribution -> Jump Hash
   - memory cost -> Jump Hash
   - Jump Hash -> tricking to use with node wieghts
   - Multi-Probe consistent hashing (Google)
   - **replication**
      - for protect node failure
      - reduce tail latency
      - choose secondary nodes for storage (full or partial replication)
   - weighted hosts
   - **loading balancing**
      - sometimes no better than random assignment -> still unbalanced distribution
      - "Consistent Hashing with Bounded Loads" -> load is checked & skipped
      - might combine with other load distribution techniques (round-robin, etc)