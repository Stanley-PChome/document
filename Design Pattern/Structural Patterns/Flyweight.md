```
享元模式（Flyweight Pattern）是一種結構型設計模式，用於減少物件創建時的內存佔用。它通過共享相似對象的方式，
來降低大量相同對象的內存消耗。享元模式特別適用於具有大量細粒度相似對象的場景，比如需要處理大量重複數據或常用狀態的情況。

模式結構與概念
  享元接口（Flyweight Interface）：
    定義享元對象的通用方法，提供外部狀態的處理方式。
  具體享元（Concrete Flyweight）：
    實現享元接口並存儲內部狀態（不變的部分）。具體享元對象之間可以共享內部狀態來節省內存。
  非共享享元（Unshared Concrete Flyweight）：
  在某些情況下，享元模式中存在不可以共享的對象（如某些特定狀態的對象），這些對象並不與其他對象共享。
  享元工廠（Flyweight Factory）：
    管理並創建享元對象，通過共享的方式來確保相同的對象只創建一次。
  外部狀態（Extrinsic State）：
    外部狀態是與享元對象的內部狀態無關的部分，需要在運行時進行區分。

優點與缺點
  優點
    減少內存消耗：
      通過共享相同對象來節省內存，特別適合於具有大量相似對象的場景。
    提升效能：
      由於減少了重複對象的創建，享元模式可以提高系統效能，尤其是在處理大量細粒度對象時。
    降低系統複雜度：
      享元模式將內部狀態與外部狀態分離，簡化了系統設計。
  
  缺點
    引入外部狀態的管理：
      享元模式要求將對象的內部狀態與外部狀態分離，這會增加系統在外部狀態管理上的複雜度。
    不適用於狀態變化頻繁的對象：
      如果對象狀態變化過於頻繁或需要在外部進行修改，享元模式的共享特性將失去其優勢。
    難以應對大量不同組合狀態：
      當內部狀態的組合過多時，可能需要創建過多的共享對象，從而降低了享元模式的效果。

應用情境
  圖形系統：
    在圖形繪製系統中，可以使用享元模式來重用相似的圖形對象（如相同顏色或形狀的圖形），而僅修改其位置或大小。
  文字處理系統：
    在文字處理系統中，每個字元可以被視為一個享元對象，不同的字元僅需要共享一個實例，並在顯示時指定其座標位置。
  遊戲中的角色管理：
    在遊戲開發中，可以使用享元模式來管理大量具有相同屬性的遊戲角色（如相同外觀的 NPC），僅修改其位置信息來實現多個角色的渲染。
  數據快取系統：
    在需要存儲大量重複數據的場景中，可以使用享元模式來減少數據的存儲量，通過共享相同的數據來降低內存使用。

```
```php
// 享元接口（Flyweight Interface）
interface Shape {
    public function draw($x, $y);
}

// 具體享元（Concrete Flyweight）：圓形類
class Circle implements Shape {
    private $color; // 內部狀態：顏色
    private $radius = 5; // 所有圓形的半徑相同（假設）

    public function __construct($color) {
        $this->color = $color;
    }

    // 實現享元接口中的方法，處理外部狀態（x 和 y 座標）
    public function draw($x, $y) {
        echo "Drawing a {$this->color} circle at ({$x}, {$y}), radius = {$this->radius}\n";
    }
}

// 享元工廠（Flyweight Factory）：管理享元對象
class ShapeFactory {
    private $circleMap = []; // 存放共享的圓形對象

    // 獲取共享的圓形對象
    public function getCircle($color) {
        if (!isset($this->circleMap[$color])) {
            $this->circleMap[$color] = new Circle($color);
            echo "Creating a circle of color: {$color}\n";
        }
        return $this->circleMap[$color];
    }
}

// 客戶端代碼（Client Code）
function main() {
    $factory = new ShapeFactory();

    // 創建並繪製三個不同位置的紅色圓形
    $redCircle1 = $factory->getCircle("Red");
    $redCircle1->draw(10, 20);

    $redCircle2 = $factory->getCircle("Red");
    $redCircle2->draw(30, 40);

    $redCircle3 = $factory->getCircle("Red");
    $redCircle3->draw(50, 60);

    // 創建並繪製兩個不同位置的藍色圓形
    $blueCircle1 = $factory->getCircle("Blue");
    $blueCircle1->draw(70, 80);

    $blueCircle2 = $factory->getCircle("Blue");
    $blueCircle2->draw(90, 100);
}

// 執行主函數
main();

```
```php
<?php

namespace RefactoringGuru\Flyweight\RealWorld;

/**
 * Flyweight objects represent the data shared by multiple Cat objects. This is
 * the combination of breed, color, texture, etc.
 */
class CatVariation
{
    /**
     * The so-called "intrinsic" state.
     */
    public $breed;

    public $image;

    public $color;

    public $texture;

    public $fur;

    public $size;

    public function __construct(
        string $breed,
        string $image,
        string $color,
        string $texture,
        string $fur,
        string $size
    ) {
        $this->breed = $breed;
        $this->image = $image;
        $this->color = $color;
        $this->texture = $texture;
        $this->fur = $fur;
        $this->size = $size;
    }

    /**
     * This method displays the cat information. The method accepts the
     * extrinsic  state as arguments. The rest of the state is stored inside
     * Flyweight's fields.
     *
     * You might be wondering why we had put the primary cat's logic into the
     * CatVariation class instead of keeping it in the Cat class. I agree, it
     * does sound confusing.
     *
     * Keep in mind that in the real world, the Flyweight pattern can either be
     * implemented from the start or forced onto an existing application
     * whenever the developers realize they've hit upon a RAM problem.
     *
     * In the latter case, you end up with such classes as we have here. We kind
     * of "refactored" an ideal app where all the data was initially inside the
     * Cat class. If we had implemented the Flyweight from the start, our class
     * names might be different and less confusing. For example, Cat and
     * CatContext.
     *
     * However, the actual reason why the primary behavior should live in the
     * Flyweight class is that you might not have the Context class declared at
     * all. The context data might be stored in an array or some other more
     * efficient data structure. You won't have another place to put your
     * methods in, except the Flyweight class.
     */
    public function renderProfile(string $name, string  $age, string $owner)
    {
        echo "= $name =\n";
        echo "Age: $age\n";
        echo "Owner: $owner\n";
        echo "Breed: $this->breed\n";
        echo "Image: $this->image\n";
        echo "Color: $this->color\n";
        echo "Texture: $this->texture\n";
    }
}

/**
 * The context stores the data unique for each cat.
 *
 * A designated class for storing context is optional and not always viable. The
 * context may be stored inside a massive data structure within the Client code
 * and passed to the flyweight methods when needed.
 */
class Cat
{
    /**
     * The so-called "extrinsic" state.
     */
    public $name;

    public $age;

    public $owner;

    /**
     * @var CatVariation
     */
    private $variation;

    public function __construct(string $name, string $age, string $owner, CatVariation $variation)
    {
        $this->name = $name;
        $this->age = $age;
        $this->owner = $owner;
        $this->variation = $variation;
    }

    /**
     * Since the Context objects don't own all of their state, sometimes, for
     * the sake of convenience, you may need to implement some helper methods
     * (for example, for comparing several Context objects.)
     */
    public function matches(array $query): bool
    {
        foreach ($query as $key => $value) {
            if (property_exists($this, $key)) {
                if ($this->$key != $value) {
                    return false;
                }
            } elseif (property_exists($this->variation, $key)) {
                if ($this->variation->$key != $value) {
                    return false;
                }
            } else {
                return false;
            }
        }

        return true;
    }

    /**
     * The Context might also define several shortcut methods, that delegate
     * execution to the Flyweight object. These methods might be remnants of
     * real methods, extracted to the Flyweight class during a massive
     * refactoring to the Flyweight pattern.
     */
    public function render(): string
    {
        $this->variation->renderProfile($this->name, $this->age, $this->owner);
    }
}

/**
 * The Flyweight Factory stores both the Context and Flyweight objects,
 * effectively hiding any notion of the Flyweight pattern from the client.
 */
class CatDataBase
{
    /**
     * The list of cat objects (Contexts).
     */
    private $cats = [];

    /**
     * The list of cat variations (Flyweights).
     */
    private $variations = [];

    /**
     * When adding a cat to the database, we look for an existing cat variation
     * first.
     */
    public function addCat(
        string $name,
        string $age,
        string $owner,
        string $breed,
        string $image,
        string $color,
        string $texture,
        string $fur,
        string $size
    ) {
        $variation =
            $this->getVariation($breed, $image, $color, $texture, $fur, $size);
        $this->cats[] = new Cat($name, $age, $owner, $variation);
        echo "CatDataBase: Added a cat ($name, $breed).\n";
    }

    /**
     * Return an existing variation (Flyweight) by given data or create a new
     * one if it doesn't exist yet.
     */
    public function getVariation(
        string $breed,
        string $image, $color,
        string $texture,
        string $fur,
        string $size
    ): CatVariation {
        $key = $this->getKey(get_defined_vars());

        if (!isset($this->variations[$key])) {
            $this->variations[$key] =
                new CatVariation($breed, $image, $color, $texture, $fur, $size);
        }

        return $this->variations[$key];
    }

    /**
     * This function helps to generate unique array keys.
     */
    private function getKey(array $data): string
    {
        return md5(implode("_", $data));
    }

    /**
     * Look for a cat in the database using the given query parameters.
     */
    public function findCat(array $query)
    {
        foreach ($this->cats as $cat) {
            if ($cat->matches($query)) {
                return $cat;
            }
        }
        echo "CatDataBase: Sorry, your query does not yield any results.";
    }
}

/**
 * The client code.
 */
$db = new CatDataBase();

echo "Client: Let's see what we have in \"cats.csv\".\n";

// To see the real effect of the pattern, you should have a large database with
// several millions of records. Feel free to experiment with code to see the
// real extent of the pattern.
$handle = fopen(__DIR__ . "/cats.csv", "r");
$row = 0;
$columns = [];
while (($data = fgetcsv($handle)) !== false) {
    if ($row == 0) {
        for ($c = 0; $c < count($data); $c++) {
            $columnIndex = $c;
            $columnKey = strtolower($data[$c]);
            $columns[$columnKey] = $columnIndex;
        }
        $row++;
        continue;
    }

    $db->addCat(
        $data[$columns['name']],
        $data[$columns['age']],
        $data[$columns['owner']],
        $data[$columns['breed']],
        $data[$columns['image']],
        $data[$columns['color']],
        $data[$columns['texture']],
        $data[$columns['fur']],
        $data[$columns['size']],
    );
    $row++;
}
fclose($handle);

// ...

echo "\nClient: Let's look for a cat named \"Siri\".\n";
$cat = $db->findCat(['name' => "Siri"]);
if ($cat) {
    $cat->render();
}

echo "\nClient: Let's look for a cat named \"Bob\".\n";
$cat = $db->findCat(['name' => "Bob"]);
if ($cat) {
    $cat->render();
}
```
