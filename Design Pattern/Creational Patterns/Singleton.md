```
Singleton（單例模式） 是一種創建型設計模式，旨在確保一個類只有一個實例，並提供一個全局訪問點來訪問這個實例。它通常用於
管理共享的資源（例如，配置類、日誌管理、連接池等），避免多個實例引起的數據不一致或資源浪費

結構與概念
    私有靜態屬性：
        用來存儲 Singleton 類別的唯一實例。
    私有建構子：
        防止外部通過 new 關鍵字來創建類別的實例。
    靜態方法（getInstance()）：
        負責檢查類別的靜態屬性是否已經初始化。如果沒有，則創建一個實例並返回它；如果有，則直接返回現有的實例。
    防止實例被克隆：
        在 PHP 中可以使用 __clone() 來阻止對象被複製。
    防止對象被反序列化：
        通過 __wakeup() 方法阻止從反序列化中創建新實例。

優點與缺點
    優點：
        全局訪問點：
            提供了一個可以在整個程序中使用的唯一實例。
        懶加載（Lazy Loading）：
            只有在需要時才創建實例，節省系統資源。
        避免重複實例化：
            保證同一類別只創建一個實例，減少資源浪費。
    缺點：
        難以擴展：
            如果需要修改 Singleton 類別的行為，通常會影響所有依賴於它的代碼。
        違反單一責任原則：
            單例不僅管理其自身的狀態，還控制其生命週期，可能會造成過度耦合。
        測試困難：
            由於 Singleton 的唯一性，在單元測試中難以進行替換或模擬。

 應用情境
    管理全局設定或狀態（如配置類別、記錄日誌）。
    控制對資源的訪問（如資料庫連接、文件系統訪問）。
    管理共享資料或全局服務。

```

```php
<?php

namespace RefactoringGuru\Singleton\RealWorld;

/**
 * If you need to support several types of Singletons in your app, you can
 * define the basic features of the Singleton in a base class, while moving the
 * actual business logic (like logging) to subclasses.
 */
class Singleton
{
    /**
     * The actual singleton's instance almost always resides inside a static
     * field. In this case, the static field is an array, where each subclass of
     * the Singleton stores its own instance.
     */
    private static $instances = [];

    /**
     * Singleton's constructor should not be public. However, it can't be
     * private either if we want to allow subclassing.
     */
    protected function __construct() { }

    /**
     * The method you use to get the Singleton's instance.
     */
    public static function getInstance()
    {
        $subclass = static::class;
        if (!isset(self::$instances[$subclass])) {
            // Note that here we use the "static" keyword instead of the actual
            // class name. In this context, the "static" keyword means "the name
            // of the current class". That detail is important because when the
            // method is called on the subclass, we want an instance of that
            // subclass to be created here.

            self::$instances[$subclass] = new static();
        }
        return self::$instances[$subclass];
    }
}

/**
 * The logging class is the most known and praised use of the Singleton pattern.
 * In most cases, you need a single logging object that writes to a single log
 * file (control over shared resource). You also need a convenient way to access
 * that instance from any context of your app (global access point).
 */
class Logger extends Singleton
{
    /**
     * A file pointer resource of the log file.
     */
    private $fileHandle;

    /**
     * Since the Singleton's constructor is called only once, just a single file
     * resource is opened at all times.
     *
     * Note, for the sake of simplicity, we open the console stream instead of
     * the actual file here.
     */
    protected function __construct()
    {
        $this->fileHandle = fopen('php://stdout', 'w');
    }

    /**
     * Write a log entry to the opened file resource.
     */
    public function writeLog(string $message): void
    {
        $date = date('Y-m-d');
        fwrite($this->fileHandle, "$date: $message\n");
    }

    /**
     * Just a handy shortcut to reduce the amount of code needed to log messages
     * from the client code.
     */
    public static function log(string $message): void
    {
        $logger = static::getInstance();
        $logger->writeLog($message);
    }
}

/**
 * Applying the Singleton pattern to the configuration storage is also a common
 * practice. Often you need to access application configurations from a lot of
 * different places of the program. Singleton gives you that comfort.
 */
class Config extends Singleton
{
    private $hashmap = [];

    public function getValue(string $key): string
    {
        return $this->hashmap[$key];
    }

    public function setValue(string $key, string $value): void
    {
        $this->hashmap[$key] = $value;
    }
}

/**
 * The client code.
 */
Logger::log("Started!");

// Compare values of Logger singleton.
$l1 = Logger::getInstance();
$l2 = Logger::getInstance();
if ($l1 === $l2) {
    Logger::log("Logger has a single instance.");
} else {
    Logger::log("Loggers are different.");
}

// Check how Config singleton saves data...
$config1 = Config::getInstance();
$login = "test_login";
$password = "test_password";
$config1->setValue("login", $login);
$config1->setValue("password", $password);
// ...and restores it.
$config2 = Config::getInstance();
if ($login == $config2->getValue("login") &&
    $password == $config2->getValue("password")
) {
    Logger::log("Config singleton also works fine.");
}

Logger::log("Finished!");
```
