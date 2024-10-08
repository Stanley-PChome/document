```
橋接模式（Bridge Pattern）是一種結構型設計模式，旨在將抽象部分與其具體實現部分分離，
使得兩者可以獨立變化。這種模式的主要目的是通過引入橋接接口來減少類的數量，從而提高系統的靈活性和可擴展性。

模式結構與概念
  抽象類（Abstraction）：
    定義了高層次的操作，並持有對實現接口的引用。
  擴展抽象類（Refined Abstraction）：
    擴展了抽象類，並實現了一些具體的操作。
  實現接口（Implementor）：
    定義了實現部分的接口。
  具體實現類（Concrete Implementor）：
    實現了實現接口的具體類。

優點與缺點
  優點
    分離抽象與實現：
      橋接模式將抽象與具體實現分開，使得兩者可以獨立變化，降低了系統的耦合度。
    靈活性與擴展性：
      可以隨意地擴展抽象和實現，添加新的實現而無需修改客戶端代碼。
    避免類爆炸：
      通過將抽象和實現分開，減少了類的數量，避免了因類的組合導致的類爆炸問題。
  缺點
    複雜性：
      引入橋接模式會增加系統的複雜性，尤其是在簡單的系統中可能顯得多餘。
    管理難度：
      由於抽象和實現分開，可能會導致管理這些組件的難度增加。

應用情境
  多維度變化：
    當你有多個維度需要變化（例如形狀和顏色），而這些維度都需要獨立變化時，橋接模式非常有效。
  避免類爆炸：
    在需要多個實現的情況下，橋接模式可以幫助減少類的數量，從而避免類的組合導致的複雜性。
  不同平台的支持：
    當應用需要在不同平台上執行，而每個平台又有其獨特的實現時，橋接模式可以簡化這種實現。
```
```php
// Implementor Interface
interface DrawingAPI {
    public function drawCircle($x, $y, $radius);
}

// Concrete Implementor 1
class DrawingAPI1 implements DrawingAPI {
    public function drawCircle($x, $y, $radius) {
        echo "Drawing Circle (API1): Center ($x, $y), Radius: $radius\n";
    }
}

// Concrete Implementor 2
class DrawingAPI2 implements DrawingAPI {
    public function drawCircle($x, $y, $radius) {
        echo "Drawing Circle (API2): Center ($x, $y), Radius: $radius\n";
    }
}

// Abstraction
abstract class Shape {
    protected $drawingAPI;

    protected function __construct(DrawingAPI $drawingAPI) {
        $this->drawingAPI = $drawingAPI;
    }

    abstract public function draw(); // 定義抽象方法
}

// Refined Abstraction
class Circle extends Shape {
    private $x;
    private $y;
    private $radius;

    public function __construct($x, $y, $radius, DrawingAPI $drawingAPI) {
        parent::__construct($drawingAPI);
        $this->x = $x;
        $this->y = $y;
        $this->radius = $radius;
    }

    public function draw() {
        $this->drawingAPI->drawCircle($this->x, $this->y, $this->radius);
    }
}

// Client Code
function main() {
    $circle1 = new Circle(5, 10, 3, new DrawingAPI1());
    $circle1->draw(); // 使用 DrawingAPI1

    $circle2 = new Circle(15, 20, 7, new DrawingAPI2());
    $circle2->draw(); // 使用 DrawingAPI2
}

// 執行主函數
main();

```
```php
<?php

namespace RefactoringGuru\Bridge\Conceptual;

/**
 * The Abstraction defines the interface for the "control" part of the two class
 * hierarchies. It maintains a reference to an object of the Implementation
 * hierarchy and delegates all of the real work to this object.
 */
class Abstraction
{
    /**
     * @var Implementation
     */
    protected $implementation;

    public function __construct(Implementation $implementation)
    {
        $this->implementation = $implementation;
    }

    public function operation(): string
    {
        return "Abstraction: Base operation with:\n" .
            $this->implementation->operationImplementation();
    }
}

/**
 * You can extend the Abstraction without changing the Implementation classes.
 */
class ExtendedAbstraction extends Abstraction
{
    public function operation(): string
    {
        return "ExtendedAbstraction: Extended operation with:\n" .
            $this->implementation->operationImplementation();
    }
}

/**
 * The Implementation defines the interface for all implementation classes. It
 * doesn't have to match the Abstraction's interface. In fact, the two
 * interfaces can be entirely different. Typically the Implementation interface
 * provides only primitive operations, while the Abstraction defines higher-
 * level operations based on those primitives.
 */
interface Implementation
{
    public function operationImplementation(): string;
}

/**
 * Each Concrete Implementation corresponds to a specific platform and
 * implements the Implementation interface using that platform's API.
 */
class ConcreteImplementationA implements Implementation
{
    public function operationImplementation(): string
    {
        return "ConcreteImplementationA: Here's the result on the platform A.\n";
    }
}

class ConcreteImplementationB implements Implementation
{
    public function operationImplementation(): string
    {
        return "ConcreteImplementationB: Here's the result on the platform B.\n";
    }
}

/**
 * Except for the initialization phase, where an Abstraction object gets linked
 * with a specific Implementation object, the client code should only depend on
 * the Abstraction class. This way the client code can support any abstraction-
 * implementation combination.
 */
function clientCode(Abstraction $abstraction)
{
    // ...

    echo $abstraction->operation();

    // ...
}

/**
 * The client code should be able to work with any pre-configured abstraction-
 * implementation combination.
 */
$implementation = new ConcreteImplementationA();
$abstraction = new Abstraction($implementation);
clientCode($abstraction);

echo "\n";

$implementation = new ConcreteImplementationB();
$abstraction = new ExtendedAbstraction($implementation);
clientCode($abstraction);
```
