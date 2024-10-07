```
裝飾者模式（Decorator Pattern）是一種結構型設計模式，用於在不改變原始類的基礎上，通過將對象放入一個特殊封裝對象中來動態地擴展其功能。與繼承相比，裝飾者模式提供了一種更靈活的方式來增強對象的行為，允許以組合的方式來擴展對象的功能。

模式結構與概念
  組件接口（Component Interface）：
    定義原始對象和裝飾者的共同接口，使得裝飾者可以與原始對象保持一致的操作方式。
  具體組件（Concrete Component）：
    定義要裝飾的對象，即需要擴展其功能的基本對象。
  裝飾者（Decorator）：
    持有組件接口的引用，並實現相同的接口以擴展基本對象的功能。
  具體裝飾者（Concrete Decorator）：
    實現具體的裝飾行為，添加額外的功能或修改基本對象的行為。

優點與缺點
  優點
    單一責任原則：
      可以將不同的功能分離到不同的類中，使用裝飾者來動態組合，而不是在一個類中處理所有功能。
    動態擴展對象的行為：
      相比繼承，裝飾者模式更靈活，允許在運行時動態地改變對象的行為。
    避免類爆炸問題：
      通過使用不同的裝飾者來組合功能，可以避免繼承層次過深或產生大量子類的問題。
  缺點
    增加系統複雜度：
      每個裝飾者都是一個新的類，可能會增加類的數量，導致系統變得更加複雜。
    不容易排錯：
      當多個裝飾者層次組合在一起時，可能會使調試和排錯變得困難，因為你需要檢查每個裝飾者的行為。

應用情境
  需要在運行時動態改變對象的行為：
    例如，為咖啡添加不同的配料（如牛奶、糖、巧克力），使用裝飾者模式可以靈活地組合不同配料。
  避免大量子類的產生：
    當類的組合功能變得複雜時，裝飾者模式可以避免繼承層次的過度擴展。
  需要透明地增強對象功能：
    客戶端無需知道對象是原始對象還是被裝飾過的對象，只需使用統一接口進行操作。
  精細控制擴展的順序：
    裝飾者模式允許開發者控制擴展的順序，通過不同的裝飾者層次來確保功能的先後順序。
```
```php
// 組件接口（Component Interface）
interface Coffee {
    public function cost();
    public function getDescription();
}

// 具體組件（Concrete Component）
class SimpleCoffee implements Coffee {
    public function cost() {
        return 10; // 基本咖啡的價格
    }

    public function getDescription() {
        return "Simple Coffee";
    }
}

// 裝飾者（Decorator）
abstract class CoffeeDecorator implements Coffee {
    protected $coffee;

    public function __construct(Coffee $coffee) {
        $this->coffee = $coffee;
    }

    public function cost() {
        return $this->coffee->cost(); // 委託原始咖啡對象的 cost 方法
    }

    public function getDescription() {
        return $this->coffee->getDescription(); // 委託原始咖啡對象的 getDescription 方法
    }
}

// 具體裝飾者 1（Concrete Decorator 1）: 添加牛奶
class MilkDecorator extends CoffeeDecorator {
    public function cost() {
        return parent::cost() + 2; // 牛奶的額外費用
    }

    public function getDescription() {
        return parent::getDescription() . ", Milk";
    }
}

// 具體裝飾者 2（Concrete Decorator 2）: 添加糖
class SugarDecorator extends CoffeeDecorator {
    public function cost() {
        return parent::cost() + 1; // 糖的額外費用
    }

    public function getDescription() {
        return parent::getDescription() . ", Sugar";
    }
}

// 具體裝飾者 3（Concrete Decorator 3）: 添加巧克力
class ChocolateDecorator extends CoffeeDecorator {
    public function cost() {
        return parent::cost() + 3; // 巧克力的額外費用
    }

    public function getDescription() {
        return parent::getDescription() . ", Chocolate";
    }
}

// 客戶端代碼（Client Code）
function main() {
    // 基本咖啡
    $coffee = new SimpleCoffee();
    echo $coffee->getDescription() . " Cost: $" . $coffee->cost() . "\n";

    // 添加牛奶的咖啡
    $milkCoffee = new MilkDecorator($coffee);
    echo $milkCoffee->getDescription() . " Cost: $" . $milkCoffee->cost() . "\n";

    // 添加牛奶和糖的咖啡
    $milkSugarCoffee = new SugarDecorator($milkCoffee);
    echo $milkSugarCoffee->getDescription() . " Cost: $" . $milkSugarCoffee->cost() . "\n";

    // 添加牛奶、糖和巧克力的咖啡
    $fullOptionCoffee = new ChocolateDecorator($milkSugarCoffee);
    echo $fullOptionCoffee->getDescription() . " Cost: $" . $fullOptionCoffee->cost() . "\n";
}

// 執行主函數
main();

```
```php
```
