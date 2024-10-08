```
外觀模式（Facade Pattern）是一種結構型設計模式，提供了一個統一的介面，用來簡化子系統中各種複雜對象的操作。
通過外觀模式，可以讓客戶端使用簡單的接口來調用多個子系統，而不需要了解這些子系統內部的實現細節。外觀模式的主要目的是減少系統間的耦合，並提供一個簡化的使用介面。

模式結構與概念
  外觀類（Facade）：
    提供一個簡化的介面來訪問系統中多個子系統的功能。這個介面將所有的子系統功能封裝起來，統一對外提供簡單的接口。
  子系統類（Subsystem Classes）：
    系統中各個子模組或功能組件，它們具有複雜的操作方式或彼此之間存在依賴關係。客戶端無需直接訪問子系統類，而是通過外觀類進行操作。
  客戶端（Client）：
    調用外觀類的接口來完成操作，而不需要知道具體的子系統如何工作。

優點與缺點
  優點
    簡化接口：
      外觀模式提供了一個簡單易用的接口，減少了客戶端與子系統之間的交互，降低了系統的複雜度。
    降低耦合性：
      通過引入外觀類，可以隔離客戶端與子系統之間的依賴關係，使得子系統的修改不會影響客戶端。
    更好的分層設計：
      外觀模式促進了分層結構的使用，有助於實現高內聚、低耦合的系統設計。
    統一接口：
      外觀模式允許將多個子系統的功能組合起來，形成一個統一的高級接口供客戶端使用。
  缺點
    可能成為上帝對象：
      如果外觀類封裝了過多的子系統功能，可能會演變為「上帝對象」（God Object），導致外觀類過於龐大，難以維護。
    隱藏子系統行為：
      外觀模式可能會過度簡化系統，導致客戶端無法使用子系統的特定功能，而只能通過外觀提供的有限接口操作。

應用情境
  為複雜的子系統提供簡化接口：
    當系統中存在多個子系統且交互邏輯較為複雜時，可以使用外觀模式來提供統一的簡化接口。
  減少依賴性和耦合性：
    使用外觀模式來降低客戶端與子系統的依賴性，避免客戶端與子系統之間的直接交互。
  分層架構中作為入口點：
    在分層架構中，外觀模式可以用作每層之間的入口點，確保每層之間的依賴關係最小化。
  模塊化系統中進行封裝：
    當系統功能模塊化明確時，可以使用外觀模式封裝各個模塊的複雜功能，僅向外界暴露簡單接口。
```
```php
// 子系統類（Subsystem Class）1：庫存系統
class Inventory {
    public function checkStock($productId) {
        echo "Checking stock for product: {$productId}\n";
        return true; // 假設產品都在庫
    }
}

// 子系統類（Subsystem Class）2：支付系統
class Payment {
    public function processPayment($amount) {
        echo "Processing payment of amount: $amount\n";
        return true; // 假設支付成功
    }
}

// 子系統類（Subsystem Class）3：物流系統
class Shipping {
    public function arrangeShipping($productId) {
        echo "Arranging shipping for product: {$productId}\n";
        return true; // 假設物流安排成功
    }
}

// 外觀類（Facade Class）：訂單系統
class OrderFacade {
    protected $inventory;
    protected $payment;
    protected $shipping;

    public function __construct() {
        $this->inventory = new Inventory();
        $this->payment = new Payment();
        $this->shipping = new Shipping();
    }

    // 對外提供簡化的介面
    public function placeOrder($productId, $amount) {
        echo "Starting order processing...\n";

        // 1. 檢查庫存
        if (!$this->inventory->checkStock($productId)) {
            echo "Product is out of stock.\n";
            return false;
        }

        // 2. 處理支付
        if (!$this->payment->processPayment($amount)) {
            echo "Payment failed.\n";
            return false;
        }

        // 3. 安排物流
        if (!$this->shipping->arrangeShipping($productId)) {
            echo "Shipping arrangement failed.\n";
            return false;
        }

        echo "Order placed successfully!\n";
        return true;
    }
}

// 客戶端代碼（Client Code）
function main() {
    // 使用外觀模式處理訂單
    $orderFacade = new OrderFacade();
    $orderFacade->placeOrder("Product-123", 50); // 訂購產品 ID 為 Product-123，金額為 50 的產品
}

// 執行主函數
main();

```
```php
<?php

namespace RefactoringGuru\Facade\RealWorld;

/**
 * The Facade provides a single method for downloading videos from YouTube. This
 * method hides all the complexity of the PHP network layer, YouTube API and the
 * video conversion library (FFmpeg).
 */
class YouTubeDownloader
{
    protected $youtube;
    protected $ffmpeg;

    /**
     * It is handy when the Facade can manage the lifecycle of the subsystem it
     * uses.
     */
    public function __construct(string $youtubeApiKey)
    {
        $this->youtube = new YouTube($youtubeApiKey);
        $this->ffmpeg = new FFMpeg();
    }

    /**
     * The Facade provides a simple method for downloading video and encoding it
     * to a target format (for the sake of simplicity, the real-world code is
     * commented-out).
     */
    public function downloadVideo(string $url): void
    {
        echo "Fetching video metadata from youtube...\n";
        // $title = $this->youtube->fetchVideo($url)->getTitle();
        echo "Saving video file to a temporary file...\n";
        // $this->youtube->saveAs($url, "video.mpg");

        echo "Processing source video...\n";
        // $video = $this->ffmpeg->open('video.mpg');
        echo "Normalizing and resizing the video to smaller dimensions...\n";
        // $video
        //     ->filters()
        //     ->resize(new FFMpeg\Coordinate\Dimension(320, 240))
        //     ->synchronize();
        echo "Capturing preview image...\n";
        // $video
        //     ->frame(FFMpeg\Coordinate\TimeCode::fromSeconds(10))
        //     ->save($title . 'frame.jpg');
        echo "Saving video in target formats...\n";
        // $video
        //     ->save(new FFMpeg\Format\Video\X264(), $title . '.mp4')
        //     ->save(new FFMpeg\Format\Video\WMV(), $title . '.wmv')
        //     ->save(new FFMpeg\Format\Video\WebM(), $title . '.webm');
        echo "Done!\n";
    }
}

/**
 * The YouTube API subsystem.
 */
class YouTube
{
    public function fetchVideo(): string { /* ... */ }

    public function saveAs(string $path): void { /* ... */ }

    // ...more methods and classes...
}

/**
 * The FFmpeg subsystem (a complex video/audio conversion library).
 */
class FFMpeg
{
    public static function create(): FFMpeg { /* ... */ }

    public function open(string $video): void { /* ... */ }

    // ...more methods and classes... RU: ...дополнительные методы и классы...
}

class FFMpegVideo
{
    public function filters(): self { /* ... */ }

    public function resize(): self { /* ... */ }

    public function synchronize(): self { /* ... */ }

    public function frame(): self { /* ... */ }

    public function save(string $path): self { /* ... */ }

    // ...more methods and classes... RU: ...дополнительные методы и классы...
}


/**
 * The client code does not depend on any subsystem's classes. Any changes
 * inside the subsystem's code won't affect the client code. You will only need
 * to update the Facade.
 */
function clientCode(YouTubeDownloader $facade)
{
    // ...

    $facade->downloadVideo("https://www.youtube.com/watch?v=QH2-TGUlwu4");

    // ...
}

$facade = new YouTubeDownloader("APIKEY-XXXXXXXXX");
clientCode($facade);
```
