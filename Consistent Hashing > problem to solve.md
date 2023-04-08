- in traditional "HashMap implementations", most keys are expect to move in node introduction/removal
   - when implement in "applications" -> **classic mod-N hashing**

---

- target environment with less total bucket (server) amount
   - **add/remove node**
      - Hash Table -> O(key-amount)
      - Consistent Hashing -> O(K/N + log N)
         - additional log N -> cost to maintain the sorted (hash value for) server ids
   - **key search** ~~(add/remove key)~~
      - Hash Table -> O(1)
      - Consistent Hashing -> O(log N)
         - log N -> binary search lookup time for server id seek
         - N typically being small, so cost efficient

---

- example
   - server-ids = n1, n2, n3
   - server-ids.length = n-count
   - [given] request-id = r1, r2, r3
   - destination-server-id = MD5(request-id) mod n-count

---

- consistent hashing
   - **(in reshard) only total-keys/bucket-amount keys are remapped**