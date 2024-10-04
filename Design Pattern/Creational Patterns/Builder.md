```
Builder（建造者模式） 是一種創建型設計模式，通過一步一步構建複雜對象來將對象的創建過程與其表示分離。
這種模式適合用於構建複雜的物件，尤其是當物件有多種組合方式時。Builder 模式允許使用者透過不同的構建
步驟（如逐步設置屬性）來創建不同表示的物件。

優點：
  解耦建構和表示：
    將建構邏輯（即如何一步一步構建對象）與對象的表示分離，易於擴展不同的產品表示。
  細粒度控制：
    可以通過 Builder 實現對對象創建過程的細粒度控制（如根據不同條件選擇不同的建構步驟）。
  提升可讀性：
    使用清晰的方法名稱來一步步構建對象，使代碼更加易讀。
  易於擴展：
    當需要構建新類型的產品時，只需創建新的 Concrete Builder 類別即可，無需修改現有代碼。

缺點：
  代碼量增加：
    相對於直接創建物件，Builder 模式的設計較為複雜，增加了類別和接口數量。
  產品構建順序受限：
    Director 的建構順序是固定的，如果需要靈活地變更順序，需要額外調整 Director 的實現。

Builder 模式的實際應用情境
  構建複雜物件（如房屋、車輛、計算機等）：
    當物件包含多個組件且組件間的組合方式多樣時，使用 Builder 模式可以更加靈活地創建不同配置的物件。
  生成不同類型的文件（如 PDF、Word、HTML）：
    將內容、格式和其他設置分離，使得生成不同格式的文件更易於管理。
  遊戲角色創建系統：
    在 RPG 類遊戲中，可以用 Builder 模式來創建不同類型的角色（如戰士、法師），透過不同的組件來配置角色屬性（如生命值、魔法、攻擊力）。
```

```php
<?php
// 1. 定義 Product 類別：Computer
class Computer {
    private $CPU;
    private $RAM;
    private $storage;
    private $graphicCard;

    // 設定 CPU
    public function setCPU($CPU) {
        $this->CPU = $CPU;
    }

    // 設定 RAM
    public function setRAM($RAM) {
        $this->RAM = $RAM;
    }

    // 設定儲存裝置
    public function setStorage($storage) {
        $this->storage = $storage;
    }

    // 設定顯示卡
    public function setGraphicCard($graphicCard) {
        $this->graphicCard = $graphicCard;
    }

    // 顯示電腦配置
    public function showSpecs() {
        echo "Computer Specs:\n";
        echo "CPU: " . $this->CPU . "\n";
        echo "RAM: " . $this->RAM . "\n";
        echo "Storage: " . $this->storage . "\n";
        echo "Graphic Card: " . $this->graphicCard . "\n";
    }
}

// 2. 定義 Builder 抽象接口
interface ComputerBuilder {
    public function addCPU();
    public function addRAM();
    public function addStorage();
    public function addGraphicCard();
    public function getComputer();
}

// 3. 具體的 Builder 類別：GamingComputerBuilder（遊戲電腦建造者）
class GamingComputerBuilder implements ComputerBuilder {
    private $computer;

    public function __construct() {
        $this->computer = new Computer();
    }

    public function addCPU() {
        $this->computer->setCPU("Intel i9-12900K");
    }

    public function addRAM() {
        $this->computer->setRAM("32GB DDR5");
    }

    public function addStorage() {
        $this->computer->setStorage("1TB NVMe SSD");
    }

    public function addGraphicCard() {
        $this->computer->setGraphicCard("NVIDIA RTX 3080");
    }

    public function getComputer() {
        return $this->computer;
    }
}

// 4. 具體的 Builder 類別：OfficeComputerBuilder（辦公電腦建造者）
class OfficeComputerBuilder implements ComputerBuilder {
    private $computer;

    public function __construct() {
        $this->computer = new Computer();
    }

    public function addCPU() {
        $this->computer->setCPU("Intel i5-12600K");
    }

    public function addRAM() {
        $this->computer->setRAM("16GB DDR4");
    }

    public function addStorage() {
        $this->computer->setStorage("512GB SSD");
    }

    public function addGraphicCard() {
        $this->computer->setGraphicCard("Integrated Graphics");
    }

    public function getComputer() {
        return $this->computer;
    }
}

// 5. Director 類別：負責構建特定順序
class Director {
    private $builder;

    public function setBuilder(ComputerBuilder $builder) {
        $this->builder = $builder;
    }

    public function buildComputer() {
        $this->builder->addCPU();
        $this->builder->addRAM();
        $this->builder->addStorage();
        $this->builder->addGraphicCard();
    }
}

// 6. 使用 Builder 模式來構建不同的電腦類別
$director = new Director();

// 構建遊戲電腦
$gamingBuilder = new GamingComputerBuilder();
$director->setBuilder($gamingBuilder);
$director->buildComputer();
$gamingComputer = $gamingBuilder->getComputer();
$gamingComputer->showSpecs();  // 輸出：遊戲電腦的配置

echo "\n";

// 構建辦公電腦
$officeBuilder = new OfficeComputerBuilder();
$director->setBuilder($officeBuilder);
$director->buildComputer();
$officeComputer = $officeBuilder->getComputer();
$officeComputer->showSpecs();  // 輸出：辦公電腦的配置

```
