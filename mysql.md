```
MySQL 隔離級別有以下幾種：

讀未提交（Read Uncommitted）
  最低的隔離級別，允許讀取其他事務尚未提交的變更，可能導致髒讀（Dirty Read）。
  
讀已提交（Read Committed）
  保證只能讀取其他事務已提交的數據，避免髒讀，但可能出現不可重複讀（Non-repeatable Read）。

可重複讀（Repeatable Read）
  保證同一事務內多次讀取相同數據的結果一致，避免髒讀與不可重複讀，但可能出現幻讀（Phantom Read）。
  MySQL InnoDB 的默認隔離級別。
  
可序列化（Serializable）
  最高的隔離級別，通過強制事務順序執行，完全避免髒讀、不可重複讀與幻讀，但可能大幅降低並發性能。

```
