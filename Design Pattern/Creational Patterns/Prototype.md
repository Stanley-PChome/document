```
Prototype（原型模式） 是一種創建型設計模式，允許你通過複製現有的物件來創建新物件，而不是通過 new 關鍵字來創建新的實例。這樣可以節省創建物件的開銷，尤其是當物件初始化成本高昂時非常有用。
```

```php
<?php

// 1. 定義 Prototype 接口（或抽象類別）
abstract class BookPrototype {
    protected $title;
    protected $author;

    // 設定書名
    public function setTitle($title) {
        $this->title = $title;
    }

    // 取得書名
    public function getTitle() {
        return $this->title;
    }

    // 強制實現 clone 方法
    abstract public function __clone();
}

// 2. 具體 Prototype 實現類別
class ScienceBookPrototype extends BookPrototype {
    // 設定此類型書的屬性（這裡只是舉例）
    protected $category = "Science";

    // 實現 clone 方法
    public function __clone() {
        // 可根據需要進行深層複製或初始化
        echo "Cloning a science book: {$this->title}\n";
    }
}

class HistoryBookPrototype extends BookPrototype {
    protected $category = "History";

    public function __clone() {
        echo "Cloning a history book: {$this->title}\n";
    }
}

// 3. 使用 Prototype 模式來創建對象
$scienceBookPrototype = new ScienceBookPrototype();
$historyBookPrototype = new HistoryBookPrototype();

// 設置原型書的屬性
$scienceBookPrototype->setTitle("A Brief History of Time");
$historyBookPrototype->setTitle("The History of the Ancient World");

// 通過 clone 來創建新書籍
$book1 = clone $scienceBookPrototype;
$book2 = clone $historyBookPrototype;

// 自訂新書名
$book1->setTitle("The Universe in a Nutshell");
$book2->setTitle("Guns, Germs, and Steel");

// 測試輸出
echo $book1->getTitle() . "\n";  // The Universe in a Nutshell
echo $book2->getTitle() . "\n";  // Guns, Germs, and Steel

```
