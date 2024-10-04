```
適配器模式（Adapter Pattern）是一種結構型設計模式，用於將一個類的接口轉換為客戶期望的另一個接口。
這使得原本因接口不兼容而無法協作的類可以一起工作。適配器模式的主要目的是提高系統的靈活性和可重用性。

模式結構與概念
  適配器模式的結構主要包括以下幾個組件：
    目標接口（Target Interface）：
      定義客戶端所期望的接口。
    適配器（Adapter）：
      實現目標接口，並調用被適配的類（Adaptee）的方法。
    現有類（Adaptee）：
      一個已存在的類，其接口不兼容於目標接口。
    客戶端（Client）：
      使用目標接口的代碼。

優點與缺點
  優點
    解耦：
      適配器模式將客戶端和現有類之間的關係解耦，使得客戶端不需要關心具體實現。
    可擴展性：
      可以輕鬆添加新的適配器以支持不同的接口，而不需要修改客戶端代碼。
    重用性：
      可以重用已有的類，無需改變其代碼。
  缺點
    增加複雜性：
      引入適配器會增加系統的複雜性，可能導致代碼的理解和維護變得困難。
    性能問題：
      在某些情況下，適配器的額外層次可能會導致性能下降，尤其是在需要大量適配操作的情況下。

應用情境
  整合現有系統：
    當你需要將新開發的系統與舊系統集成時，適配器模式可以幫助解決接口不兼容的問題。
  使用第三方庫：
    在使用不符合應用程序接口的第三方庫時，適配器模式可以使你的應用程序能夠輕鬆地使用這些庫。
  跨平台開發：
    當開發多個平台的應用時，適配器模式可以幫助在不同平台之間進行接口適配。
  測試與模擬：
    在單元測試中，可以使用適配器模式來模擬外部依賴，這樣可以獨立測試不同的組件。
```
```php
// Target Interface
interface MediaPlayer {
    public function play($file);
}


// Adaptee
class AudioPlayer {
    public function playAudio($file) {
        echo "Playing audio file: $file\n";
    }
}

// Adapter
class AudioPlayerAdapter implements MediaPlayer {
    protected $audioPlayer;

    public function __construct(AudioPlayer $audioPlayer) {
        $this->audioPlayer = $audioPlayer;
    }

    public function play($file) {
        $this->audioPlayer->playAudio($file);
    }
}

// Client Code
function playMedia(MediaPlayer $mediaPlayer, $file) {
    $mediaPlayer->play($file);
}

// 使用現有類
$audioPlayer = new AudioPlayer();
$adapter = new AudioPlayerAdapter($audioPlayer);

// 通過適配器播放音頻
playMedia($adapter, 'song.mp3');

```
```php
<?php

namespace RefactoringGuru\Adapter\RealWorld;

/**
 * The Target interface represents the interface that your application's classes
 * already follow.
 */
interface Notification
{
    public function send(string $title, string $message);
}

/**
 * Here's an example of the existing class that follows the Target interface.
 *
 * The truth is that many real apps may not have this interface clearly defined.
 * If you're in that boat, your best bet would be to extend the Adapter from one
 * of your application's existing classes. If that's awkward (for instance,
 * SlackNotification doesn't feel like a subclass of EmailNotification), then
 * extracting an interface should be your first step.
 */
class EmailNotification implements Notification
{
    private $adminEmail;

    public function __construct(string $adminEmail)
    {
        $this->adminEmail = $adminEmail;
    }

    public function send(string $title, string $message): void
    {
        mail($this->adminEmail, $title, $message);
        echo "Sent email with title '$title' to '{$this->adminEmail}' that says '$message'.";
    }
}

/**
 * The Adaptee is some useful class, incompatible with the Target interface. You
 * can't just go in and change the code of the class to follow the Target
 * interface, since the code might be provided by a 3rd-party library.
 */
class SlackApi
{
    private $login;
    private $apiKey;

    public function __construct(string $login, string $apiKey)
    {
        $this->login = $login;
        $this->apiKey = $apiKey;
    }

    public function logIn(): void
    {
        // Send authentication request to Slack web service.
        echo "Logged in to a slack account '{$this->login}'.\n";
    }

    public function sendMessage(string $chatId, string $message): void
    {
        // Send message post request to Slack web service.
        echo "Posted following message into the '$chatId' chat: '$message'.\n";
    }
}

/**
 * The Adapter is a class that links the Target interface and the Adaptee class.
 * In this case, it allows the application to send notifications using Slack
 * API.
 */
class SlackNotification implements Notification
{
    private $slack;
    private $chatId;

    public function __construct(SlackApi $slack, string $chatId)
    {
        $this->slack = $slack;
        $this->chatId = $chatId;
    }

    /**
     * An Adapter is not only capable of adapting interfaces, but it can also
     * convert incoming data to the format required by the Adaptee.
     */
    public function send(string $title, string $message): void
    {
        $slackMessage = "#" . $title . "# " . strip_tags($message);
        $this->slack->logIn();
        $this->slack->sendMessage($this->chatId, $slackMessage);
    }
}

/**
 * The client code can work with any class that follows the Target interface.
 */
function clientCode(Notification $notification)
{
    // ...

    echo $notification->send("Website is down!",
        "<strong style='color:red;font-size: 50px;'>Alert!</strong> " .
        "Our website is not responding. Call admins and bring it up!");

    // ...
}

echo "Client code is designed correctly and works with email notifications:\n";
$notification = new EmailNotification("developers@example.com");
clientCode($notification);
echo "\n\n";


echo "The same client code can work with other classes via adapter:\n";
$slackApi = new SlackApi("example.com", "XXXXXXXX");
$notification = new SlackNotification($slackApi, "Example.com Developers");
clientCode($notification);
```
