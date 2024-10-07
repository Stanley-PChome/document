```
代理模式（Proxy Pattern）是一種結構型設計模式，用來在不改變對象介面的情況下，控制對目標對象的訪問。
代理類（Proxy）通常用來控制對具體對象（Real Subject）的存取，並可附加額外的邏輯（如權限檢查、延遲初始化、快取、日誌記錄等）。
代理模式的主要用途包括：保護主體（如檢查權限）、延遲加載（如在需要時再創建對象）、日誌記錄和存取控制等。

模式結構與概念
  主體接口（Subject Interface）：
    定義具體主體（Real Subject）與代理（Proxy）共同的介面，使代理對象可以取代具體主體。
  具體主體（Real Subject）：
    代理所指向的真實對象，包含具體的業務邏輯。
  代理類（Proxy）：
    通過代理來控制對具體主體的訪問，可以附加額外的操作（如日誌、權限檢查、延遲加載等）。
  客戶端（Client）：
    通過代理類來調用具體主體的方法，而不直接操作具體主體。

優點與缺點
  優點
    控制對象的存取：
      代理模式可以通過代理類來控制對具體對象的訪問，比如權限檢查、日誌記錄或計量訪問次數等。
    延遲加載（Lazy Loading）：
      代理模式可以用於延遲初始化某些大型資源，如大型物件、遠程服務、複雜計算等，提升系統效能。
    保護主體：
      代理可以在訪問具體對象前做額外的檢查（如權限檢查），從而保護具體對象免於非法訪問。
    額外操作擴展：
      代理類可以在不修改具體主體的情況下，實現額外的業務邏輯（如日誌、緩存、計時等）。
  缺點
    增加系統複雜度：
      引入代理模式會增加類的數量，增加系統的複雜度，特別是在不必要使用代理的情況下。
    效能開銷：
      代理模式會引入額外的呼叫和處理邏輯，可能會對效能產生一定的影響。
    實現困難：
      在某些情況下（如分散式系統中），代理類的實現可能會變得非常複雜且難以維護。

應用情境
  虛擬代理（Virtual Proxy）：
    用於延遲初始化大型物件，只有在需要時才真正創建這些物件。
  保護代理（Protection Proxy）：
    控制對敏感對象的存取，通常用於權限控制系統中。
  遠端代理（Remote Proxy）：
    用於控制對遠端對象的存取，在分散式系統中，可以通過代理來管理遠端資源的存取。

```
```php
// 主體接口（Subject Interface）
interface Database {
    public function query($sql);
}

// 具體主體（Real Subject）：真正執行 SQL 查詢的類別
class RealDatabase implements Database {
    public function query($sql) {
        echo "Executing query: $sql\n";
        // 模擬執行 SQL 查詢（實際情況下會連接資料庫）
    }
}

// 代理類（Proxy）：為 RealDatabase 提供控制訪問的代理
class DatabaseProxy implements Database {
    private $realDatabase; // 真實的資料庫對象

    public function __construct(RealDatabase $realDatabase) {
        $this->realDatabase = $realDatabase;
    }

    // 實現與主體相同的查詢方法
    public function query($sql) {
        // 在查詢前做額外的操作（如日誌記錄）
        $this->logQuery($sql);

        // 轉發查詢至真實資料庫對象
        $this->realDatabase->query($sql);
    }

    // 日誌記錄操作
    private function logQuery($sql) {
        echo "Logging query: $sql\n";
    }
}

// 客戶端代碼（Client Code）
function main() {
    // 創建真實的資料庫對象
    $realDatabase = new RealDatabase();

    // 通過代理來訪問資料庫
    $proxy = new DatabaseProxy($realDatabase);
    $proxy->query("SELECT * FROM users"); // 查詢所有用戶資料
    $proxy->query("DELETE FROM orders WHERE order_id = 1001"); // 刪除指定訂單
}

// 執行主函數
main();

```
```php
<?php

namespace RefactoringGuru\Proxy\RealWorld;

/**
 * The Subject interface describes the interface of a real object.
 *
 * The truth is that many real apps may not have this interface clearly defined.
 * If you're in that boat, your best bet would be to extend the Proxy from one
 * of your existing application classes. If that's awkward, then extracting a
 * proper interface should be your first step.
 */
interface Downloader
{
    public function download(string $url): string;
}

/**
 * The Real Subject does the real job, albeit not in the most efficient way.
 * When a client tries to download the same file for the second time, our
 * downloader does just that, instead of fetching the result from cache.
 */
class SimpleDownloader implements Downloader
{
    public function download(string $url): string
    {
        echo "Downloading a file from the Internet.\n";
        $result = file_get_contents($url);
        echo "Downloaded bytes: " . strlen($result) . "\n";

        return $result;
    }
}

/**
 * The Proxy class is our attempt to make the download more efficient. It wraps
 * the real downloader object and delegates it the first download calls. The
 * result is then cached, making subsequent calls return an existing file
 * instead of downloading it again.
 *
 * Note that the Proxy MUST implement the same interface as the Real Subject.
 */
class CachingDownloader implements Downloader
{
    /**
     * @var SimpleDownloader
     */
    private $downloader;

    /**
     * @var string[]
     */
    private $cache = [];

    public function __construct(SimpleDownloader $downloader)
    {
        $this->downloader = $downloader;
    }

    public function download(string $url): string
    {
        if (!isset($this->cache[$url])) {
            echo "CacheProxy MISS. ";
            $result = $this->downloader->download($url);
            $this->cache[$url] = $result;
        } else {
            echo "CacheProxy HIT. Retrieving result from cache.\n";
        }
        return $this->cache[$url];
    }
}

/**
 * The client code may issue several similar download requests. In this case,
 * the caching proxy saves time and traffic by serving results from cache.
 *
 * The client is unaware that it works with a proxy because it works with
 * downloaders via the abstract interface.
 */
function clientCode(Downloader $subject)
{
    // ...

    $result = $subject->download("http://example.com/");

    // Duplicate download requests could be cached for a speed gain.

    $result = $subject->download("http://example.com/");

    // ...
}

echo "Executing client code with real subject:\n";
$realSubject = new SimpleDownloader();
clientCode($realSubject);

echo "\n";

echo "Executing the same client code with a proxy:\n";
$proxy = new CachingDownloader($realSubject);
clientCode($proxy);
```
