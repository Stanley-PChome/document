```
組合模式（Composite Pattern）是一種結構型設計模式，它允許你將對象組合成樹狀結構以表示「部分-整體」的層次結構。
組合模式使得客戶端對單個對象和組合對象的使用方式保持一致。這個模式的主要目的是讓樹形結構中的葉節點和組合節點具有相同的操作接口。

組合模式的結構主要包括以下幾個組件：
  組件（Component）：
    定義葉節點和組合節點的共同接口，宣告所有節點應具備的操作行為。
  葉節點（Leaf）：
    樹的最小單元，代表單個對象，實現 Component 中定義的操作行為。
  組合節點（Composite）：
    可以包含多個 Component 的容器，實現 Component 的操作並管理子節點。
  客戶端（Client）：
    使用 Component 接口來操控葉節點和組合節點的代碼。

優點與缺點
  優點
    樹狀結構：
      組合模式可以輕鬆地定義樹狀結構，表示部分與整體之間的層次關係。
    統一接口：
      客戶端無需區分葉節點和組合節點的類型，可以通過統一接口進行操作。
    靈活性高：
      可以動態地添加或刪除組合中的節點，結構變得更靈活。
  缺點
    複雜性：
      引入組合模式會增加系統的複雜度，特別是在樹結構較深時，管理變得困難。
    透明性問題：
      在某些情況下，客戶端無法確定操作的是單個對象還是組合對象。

應用情境
  組織結構：
    組合模式可以用來表示組織結構，例如公司、部門、員工的層次結構。
  文件系統：
    在文件系統中，文件夾可以包含文件或其他子文件夾，這是一個典型的組合模式應用。
  圖形結構：
    圖形對象（如點、線、圓等）和它們的組合（如圖形群組）也可以使用組合模式來實現。
  UI 元素：
    用於表示具有嵌套結構的 UI 元素（如面板、按鈕、文本框等），每個元素既可以是單一對象，也可以是組合對象。

```
```php
// 組件接口（Component Interface）
interface Employee {
    public function getName();
    public function getRole();
    public function displayInfo($indent = 0);
}

// 葉節點（Leaf Node）
class Developer implements Employee {
    protected $name;
    protected $role;

    public function __construct($name, $role) {
        $this->name = $name;
        $this->role = $role;
    }

    public function getName() {
        return $this->name;
    }

    public function getRole() {
        return $this->role;
    }

    public function displayInfo($indent = 0) {
        echo str_repeat(" ", $indent) . "Developer: " . $this->getName() . " (" . $this->getRole() . ")\n";
    }
}

// 葉節點（Leaf Node）
class Designer implements Employee {
    protected $name;
    protected $role;

    public function __construct($name, $role) {
        $this->name = $name;
        $this->role = $role;
    }

    public function getName() {
        return $this->name;
    }

    public function getRole() {
        return $this->role;
    }

    public function displayInfo($indent = 0) {
        echo str_repeat(" ", $indent) . "Designer: " . $this->getName() . " (" . $this->getRole() . ")\n";
    }
}

// 組合節點（Composite Node）
class Team implements Employee {
    protected $name;
    protected $employees = [];

    public function __construct($name) {
        $this->name = $name;
    }

    public function add(Employee $employee) {
        $this->employees[] = $employee;
    }

    public function remove(Employee $employee) {
        $key = array_search($employee, $this->employees);
        if ($key !== false) {
            unset($this->employees[$key]);
        }
    }

    public function getName() {
        return $this->name;
    }

    public function getRole() {
        return "Team";
    }

    public function displayInfo($indent = 0) {
        echo str_repeat(" ", $indent) . "Team: " . $this->getName() . "\n";
        foreach ($this->employees as $employee) {
            $employee->displayInfo($indent + 4);
        }
    }
}

// 客戶端代碼（Client Code）
function main() {
    // 創建葉節點
    $dev1 = new Developer("Alice", "Backend Developer");
    $dev2 = new Developer("Bob", "Frontend Developer");
    $designer = new Designer("Charlie", "UX Designer");

    // 創建組合節點
    $team1 = new Team("Development Team");
    $team1->add($dev1);
    $team1->add($dev2);

    $team2 = new Team("Design Team");
    $team2->add($designer);

    // 創建更高層的組合節點
    $company = new Team("Company");
    $company->add($team1);
    $company->add($team2);

    // 顯示組合結構
    $company->displayInfo();
}

// 執行主函數
main();

```
```php
<?php

namespace RefactoringGuru\Composite\RealWorld;

/**
 * The base Component class declares an interface for all concrete components,
 * both simple and complex.
 *
 * In our example, we'll be focusing on the rendering behavior of DOM elements.
 */
abstract class FormElement
{
    /**
     * We can anticipate that all DOM elements require these 3 fields.
     */
    protected $name;
    protected $title;
    protected $data;

    public function __construct(string $name, string $title)
    {
        $this->name = $name;
        $this->title = $title;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setData($data): void
    {
        $this->data = $data;
    }

    public function getData(): array
    {
        return $this->data;
    }

    /**
     * Each concrete DOM element must provide its rendering implementation, but
     * we can safely assume that all of them are returning strings.
     */
    abstract public function render(): string;
}

/**
 * This is a Leaf component. Like all the Leaves, it can't have any children.
 */
class Input extends FormElement
{
    private $type;

    public function __construct(string $name, string $title, string $type)
    {
        parent::__construct($name, $title);
        $this->type = $type;
    }

    /**
     * Since Leaf components don't have any children that may handle the bulk of
     * the work for them, usually it is the Leaves who do the most of the heavy-
     * lifting within the Composite pattern.
     */
    public function render(): string
    {
        return "<label for=\"{$this->name}\">{$this->title}</label>\n" .
            "<input name=\"{$this->name}\" type=\"{$this->type}\" value=\"{$this->data}\">\n";
    }
}

/**
 * The base Composite class implements the infrastructure for managing child
 * objects, reused by all Concrete Composites.
 */
abstract class FieldComposite extends FormElement
{
    /**
     * @var FormElement[]
     */
    protected $fields = [];

    /**
     * The methods for adding/removing sub-objects.
     */
    public function add(FormElement $field): void
    {
        $name = $field->getName();
        $this->fields[$name] = $field;
    }

    public function remove(FormElement $component): void
    {
        $this->fields = array_filter($this->fields, function ($child) use ($component) {
            return $child != $component;
        });
    }

    /**
     * Whereas a Leaf's method just does the job, the Composite's method almost
     * always has to take its sub-objects into account.
     *
     * In this case, the composite can accept structured data.
     *
     * @param array $data
     */
    public function setData($data): void
    {
        foreach ($this->fields as $name => $field) {
            if (isset($data[$name])) {
                $field->setData($data[$name]);
            }
        }
    }

    /**
     * The same logic applies to the getter. It returns the structured data of
     * the composite itself (if any) and all the children data.
     */
    public function getData(): array
    {
        $data = [];
        
        foreach ($this->fields as $name => $field) {
            $data[$name] = $field->getData();
        }
        
        return $data;
    }

    /**
     * The base implementation of the Composite's rendering simply combines
     * results of all children. Concrete Composites will be able to reuse this
     * implementation in their real rendering implementations.
     */
    public function render(): string
    {
        $output = "";
        
        foreach ($this->fields as $name => $field) {
            $output .= $field->render();
        }
        
        return $output;
    }
}

/**
 * The fieldset element is a Concrete Composite.
 */
class Fieldset extends FieldComposite
{
    public function render(): string
    {
        // Note how the combined rendering result of children is incorporated
        // into the fieldset tag.
        $output = parent::render();
        
        return "<fieldset><legend>{$this->title}</legend>\n$output</fieldset>\n";
    }
}

/**
 * And so is the form element.
 */
class Form extends FieldComposite
{
    protected $url;

    public function __construct(string $name, string $title, string $url)
    {
        parent::__construct($name, $title);
        $this->url = $url;
    }

    public function render(): string
    {
        $output = parent::render();
        return "<form action=\"{$this->url}\">\n<h3>{$this->title}</h3>\n$output</form>\n";
    }
}

/**
 * The client code gets a convenient interface for building complex tree
 * structures.
 */
function getProductForm(): FormElement
{
    $form = new Form('product', "Add product", "/product/add");
    $form->add(new Input('name', "Name", 'text'));
    $form->add(new Input('description', "Description", 'text'));

    $picture = new Fieldset('photo', "Product photo");
    $picture->add(new Input('caption', "Caption", 'text'));
    $picture->add(new Input('image', "Image", 'file'));
    $form->add($picture);

    return $form;
}

/**
 * The form structure can be filled with data from various sources. The Client
 * doesn't have to traverse through all form fields to assign data to various
 * fields since the form itself can handle that.
 */
function loadProductData(FormElement $form)
{
    $data = [
        'name' => 'Apple MacBook',
        'description' => 'A decent laptop.',
        'photo' => [
            'caption' => 'Front photo.',
            'image' => 'photo1.png',
        ],
    ];

    $form->setData($data);
}

/**
 * The client code can work with form elements using the abstract interface.
 * This way, it doesn't matter whether the client works with a simple component
 * or a complex composite tree.
 */
function renderProduct(FormElement $form)
{
    // ..

    echo $form->render();

    // ..
}

$form = getProductForm();
loadProductData($form);
renderProduct($form);
```
