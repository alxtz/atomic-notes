![[0524010020.svg]]

---

## Isolation 的定義

1. concurrency transactions must not affect each other

2. at highest level (serializable)
	- transactions behaves as if they were run sequentially

3. ways transactions could interfere each other

	- **read phenomenons**
		- dirty read
			- transaction 將會彼此閱讀其他 transaction 未 commit 的寫入資料
				- 讀取未 commit 的資料 -> 可能會使用到接下來被 rollback 的內容
	
		- non-repeatable read
			- 在 transaction 內執行 SELECT (read) 行為數次，回傳的結果有可能會不一樣
				- 在 read committed level 下
					- 這代表有交易/其他操作在當前 transaction 執行到一半時，就完成了
	
			- 看起來是針對「單一 record」
			- 第二次 single record select，會讀取到 column 裡不同的值（起初可為 single or range scan）
	
		- phantom read
	
			- 定義 TBD
			- 看起來是針對 range scan
			- 第二次 range scan，會取到不一致的 total rows（起初得是 range scan）

4. ANSI SQL standards

---

## 解決的問題

- [read phenomenon]
	- lost update

---

## concurrency control 的策略

1. pessimistic concurrency control
2. optimistic concurrency control
3. MVCC

---

## 能使用的工具

1.  **[isolation level] serializable**

	- 執行的就如同 sequential
	- [MySQL]
		- 在這個 level，有沒有使用 atomic update 結果是沒差的。
		- 原因是其他交易裡的 SELECT/UPDATE statement 全都會被升級成行鎖。
	- [MySQL/Postgres]
		- 在這個 level，UPDATE/DELETE 都會有機會被其他交易永久阻擋而 timeout。
		- 所以永遠都要注意在最後一個交易寫入失敗而需要 retry 的可能。

---

2. [isolation level] repeatable read

	- 「相同」的 query，必定回傳「相同」的結果
	- 「相同 query」的定義？
		- 是否 range query/single row query/range write/delete 全都能套用
		- 這個模糊性，導致了後來各家實作 RR 出現了差異，lost update 等名詞也才被相繼發明
	- atomic update 彼此間會衝突，不能在此 level 讓兩個 atomic check & set 取得互相干涉的結果

---

3. [isolation level] read committed

	- 任何的寫入/讀取，都只會接觸到 committed data
	
		- 相當於一定不會有 dirty read
	
	- [MySQL] 會有 NRR/幻讀的行為

---

4. [isolation level] read uncommitted

	- 交易讀取的到 uncommitted data
	
		- 相當於一定會有 ditry read

---

- 行鎖

	- syntax
	
		- `SELECT {row} FOR SHARE;`
		- `SELECT {row} FOR UPDATE;`
	
	- behavior
	
		- 以「選取的 row」本身來建立 Mutex Lock（互斥鎖）

---

5. MVCC-based isolation

---

## Caveats

1. 在 SQL 標準裡描述的 concurrency control，其實不明確

2. 在 MySQL 的 RR 下使用 atomic update (SET balance = balance - 10)
	- 結果會被其他交易所干涉到（這跟 PostgreSQL 的 RR 不同）
	- PostgreSQL 的 RR，兩者會出現 serialisation error

3. MySQL 預設是 RR

4. Postgres 預設是 RC

5. Postgres 的 RU 其實被實作成 RC
	- 所以 Postgres 實際上只有三種 isolation level
	- RU 本來就不實用，所以其實也沒影響很大

---

## 閱讀資料

1. Deeply understand Isolation levels and Read phenomena in MySQL & PostgreSQL
	- https://dev.to/techschoolguru/understand-isolation-levels-read-phenomena-in-mysql-postgres-c2e

2. What is the difference between Non-Repeatable Read and Phantom Read?
	- https://stackoverflow.com/questions/11043712/what-is-the-difference-between-non-repeatable-read-and-phantom-read

3. Tackling Lost Updates Problem In Database Using Stricter Transaction Isolation Level
	- https://priyankvex.com/2018/10/20/tackling-lost-updates-problem-in-database-using-better-isolation-level/

4. Database Isolation Levels explained
	- https://ssudan16.medium.com/database-isolation-levels-explained-61429c4b1e31
	  
5. Is a single update statement with "where" transaction safe?
	- https://dba.stackexchange.com/questions/175228/is-a-single-update-statement-with-where-transaction-safe
	- https://github.com/mpyw-yattemita/mysql-postgres-update-locking-and-isolation-levels

---

## 對答案

1. 資料庫交易的 Isolation https://blog.amis.com/database-transaction-isolation-a1e448a7736e
2. 對於 MySQL Repeatable Read Isolation 常見的三個誤解 https://medium.com/@chester.yw.chu/%E5%B0%8D%E6%96%BC-mysql-repeatable-read-isolation-%E5%B8%B8%E8%A6%8B%E7%9A%84%E4%B8%89%E5%80%8B%E8%AA%A4%E8%A7%A3-7a9afbac65af
3. 複習資料庫的 Isolation Level 與圖解五個常見的 Race Conditions https://medium.com/@chester.yw.chu/%E8%A4%87%E7%BF%92%E8%B3%87%E6%96%99%E5%BA%AB%E7%9A%84-isolation-level-%E8%88%87%E5%B8%B8%E8%A6%8B%E7%9A%84%E4%BA%94%E5%80%8B-race-conditions-%E5%9C%96%E8%A7%A3-16e8d472a25c
4. 紫貓 `atomic check set` vs `行鎖` vs `serializable` https://www.facebook.com/groups/backendtw/permalink/2900373730096484/?mibextid=S66gvF