```
Builder（建造者模式） 是一種創建型設計模式，通過一步一步構建複雜對象來將對象的創建過程與其表示分離。
這種模式適合用於構建複雜的物件，尤其是當物件有多種組合方式時。Builder 模式允許使用者透過不同的構建
步驟（如逐步設置屬性）來創建不同表示的物件。

Builder 模式結構與概念
  Builder 模式通常包括以下角色：
    Builder：
      抽象建造者接口，定義構建產品不同部分的方法。
    Concrete Builder：
      具體建造者，實現 Builder 中定義的方法來構建和組合產品的各部分。
    Product：
      最終要創建的複雜物件。
    Director：
      負責按照特定順序來調用 Builder 來建構產品。

Builder 模式的優點與缺點
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

```php
<?php

namespace RefactoringGuru\Builder\RealWorld;

/**
 * The Builder interface declares a set of methods to assemble an SQL query.
 *
 * All of the construction steps are returning the current builder object to
 * allow chaining: $builder->select(...)->where(...)
 */
interface SQLQueryBuilder
{
    public function select(string $table, array $fields): SQLQueryBuilder;

    public function where(string $field, string $value, string $operator = '='): SQLQueryBuilder;

    public function limit(int $start, int $offset): SQLQueryBuilder;

    // +100 other SQL syntax methods...

    public function getSQL(): string;
}

/**
 * Each Concrete Builder corresponds to a specific SQL dialect and may implement
 * the builder steps a little bit differently from the others.
 *
 * This Concrete Builder can build SQL queries compatible with MySQL.
 */
class MysqlQueryBuilder implements SQLQueryBuilder
{
    protected $query;

    protected function reset(): void
    {
        $this->query = new \stdClass();
    }

    /**
     * Build a base SELECT query.
     */
    public function select(string $table, array $fields): SQLQueryBuilder
    {
        $this->reset();
        $this->query->base = "SELECT " . implode(", ", $fields) . " FROM " . $table;
        $this->query->type = 'select';

        return $this;
    }

    /**
     * Add a WHERE condition.
     */
    public function where(string $field, string $value, string $operator = '='): SQLQueryBuilder
    {
        if (!in_array($this->query->type, ['select', 'update', 'delete'])) {
            throw new \Exception("WHERE can only be added to SELECT, UPDATE OR DELETE");
        }
        $this->query->where[] = "$field $operator '$value'";

        return $this;
    }

    /**
     * Add a LIMIT constraint.
     */
    public function limit(int $start, int $offset): SQLQueryBuilder
    {
        if (!in_array($this->query->type, ['select'])) {
            throw new \Exception("LIMIT can only be added to SELECT");
        }
        $this->query->limit = " LIMIT " . $start . ", " . $offset;

        return $this;
    }

    /**
     * Get the final query string.
     */
    public function getSQL(): string
    {
        $query = $this->query;
        $sql = $query->base;
        if (!empty($query->where)) {
            $sql .= " WHERE " . implode(' AND ', $query->where);
        }
        if (isset($query->limit)) {
            $sql .= $query->limit;
        }
        $sql .= ";";
        return $sql;
    }
}

/**
 * This Concrete Builder is compatible with PostgreSQL. While Postgres is very
 * similar to Mysql, it still has several differences. To reuse the common code,
 * we extend it from the MySQL builder, while overriding some of the building
 * steps.
 */
class PostgresQueryBuilder extends MysqlQueryBuilder
{
    /**
     * Among other things, PostgreSQL has slightly different LIMIT syntax.
     */
    public function limit(int $start, int $offset): SQLQueryBuilder
    {
        parent::limit($start, $offset);

        $this->query->limit = " LIMIT " . $start . " OFFSET " . $offset;

        return $this;
    }

    // + tons of other overrides...
}


/**
 * Note that the client code uses the builder object directly. A designated
 * Director class is not necessary in this case, because the client code needs
 * different queries almost every time, so the sequence of the construction
 * steps cannot be easily reused.
 *
 * Since all our query builders create products of the same type (which is a
 * string), we can interact with all builders using their common interface.
 * Later, if we implement a new Builder class, we will be able to pass its
 * instance to the existing client code without breaking it thanks to the
 * SQLQueryBuilder interface.
 */
function clientCode(SQLQueryBuilder $queryBuilder)
{
    // ...

    $query = $queryBuilder
        ->select("users", ["name", "email", "password"])
        ->where("age", 18, ">")
        ->where("age", 30, "<")
        ->limit(10, 20)
        ->getSQL();

    echo $query;

    // ...
}


/**
 * The application selects the proper query builder type depending on a current
 * configuration or the environment settings.
 */
// if ($_ENV['database_type'] == 'postgres') {
//     $builder = new PostgresQueryBuilder(); } else {
//     $builder = new MysqlQueryBuilder(); }
//
// clientCode($builder);


echo "Testing MySQL query builder:\n";
clientCode(new MysqlQueryBuilder());

echo "\n\n";

echo "Testing PostgresSQL query builder:\n";
clientCode(new PostgresQueryBuilder());
```
