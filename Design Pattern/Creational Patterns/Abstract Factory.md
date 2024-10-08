```
Abstract Factory（抽象工廠模式） 是一種創建型設計模式，允許在不指定具體類別的情況下，創建一組
相關或依賴的物件。此模式主要用於處理多個產品等級結構（product hierarchies），通常涉及到一系列相互關聯的產品。

結構與概念
  抽象工廠模式允許客戶端使用工廠方法創建一組「相關」的產品，而不需要了解具體產品的類別。比如，
  針對不同的使用環境或平台（如 Windows 和 Mac），我們可以通過同一抽象工廠來生產不同的 UI 元素（如按鈕、視窗等）。

  Abstract Factory（抽象工廠）：
    定義了所有產品創建的方法。
  Concrete Factory（具體工廠）：
    實現抽象工廠的方法，負責具體產品物件的生成。
  Abstract Product（抽象產品）：
    定義所有具體產品的共同接口。
  Concrete Product（具體產品）：
    具體產品的實現類別。
  Client（客戶端）：
    通過抽象工廠來取得抽象產品的具體實例，而不需要知道具體工廠和具體產品的類別。

優點與缺點
  優點：
    符合開放封閉原則（OCP）：
      新增產品系列時，不需要更改現有的工廠代碼。
    封裝產品族：
      抽象工廠創建的是一組相關的產品，確保一個工廠生成的產品是相互兼容的。
    減少耦合性：
      客戶端代碼通過抽象接口與具體工廠互動，避免了直接依賴具體產品類別。
  缺點：
    類別數量增多：
      每個產品族都需要創建相應的工廠類別，類別數量可能會迅速增長。
    難以擴展新產品：
      如果需要在同一產品族中新增產品，則必須修改所有工廠接口及其具體實現。

使用情境
  需要創建多個相關的物件時：
      如 UI 工具包中的按鈕、文字框、視窗等。
  希望保證一組物件的一致性時：
      如需要創建不同主題風格的 UI 元素時。
  需要隔離物件的生成邏輯：
      避免直接使用 new 關鍵字創建具體產品，從而提高系統的靈活性。

與其他設計模式的結合
  Factory Method（工廠方法模式）：
    通常會在 Abstract Factory 中使用多個 Factory Method 來創建具體產品。
  Singleton（單例模式）：
    具體工廠通常被設計成單例，以確保所有工廠的使用者都能得到相同的工廠實例。
```

```php
<?php
// 1. 抽象產品：Button 介面
interface Button {
    public function render();
}

// 2. 抽象產品：TextBox 介面
interface TextBox {
    public function render();
}

// 3. 具體產品：深色主題按鈕
class DarkButton implements Button {
    public function render() {
        echo "Render a dark button.\n";
    }
}

// 4. 具體產品：深色主題文字框
class DarkTextBox implements TextBox {
    public function render() {
        echo "Render a dark text box.\n";
    }
}

// 5. 具體產品：淺色主題按鈕
class LightButton implements Button {
    public function render() {
        echo "Render a light button.\n";
    }
}

// 6. 具體產品：淺色主題文字框
class LightTextBox implements TextBox {
    public function render() {
        echo "Render a light text box.\n";
    }
}

// 7. 抽象工廠：UIFactory 介面
interface UIFactory {
    public function createButton(): Button;
    public function createTextBox(): TextBox;
}

// 8. 具體工廠：深色主題工廠
class DarkThemeFactory implements UIFactory {
    public function createButton(): Button {
        return new DarkButton();
    }

    public function createTextBox(): TextBox {
        return new DarkTextBox();
    }
}

// 9. 具體工廠：淺色主題工廠
class LightThemeFactory implements UIFactory {
    public function createButton(): Button {
        return new LightButton();
    }

    public function createTextBox(): TextBox {
        return new LightTextBox();
    }
}

// 10. 客戶端代碼：通過抽象工廠來創建 UI 元素
function renderUIComponents(UIFactory $factory) {
    $button = $factory->createButton();
    $textbox = $factory->createTextBox();

    $button->render();  // Render 按鈕
    $textbox->render(); // Render 文字框
}

// 測試範例
echo "Rendering dark theme components:\n";
renderUIComponents(new DarkThemeFactory());  // 輸出：Render a dark button. Render a dark text box.

echo "\n";

echo "Rendering light theme components:\n";
renderUIComponents(new LightThemeFactory()); // 輸出：Render a light button. Render a light text box.
?>

```
```php
<?php

namespace RefactoringGuru\AbstractFactory\RealWorld;

/**
 * The Abstract Factory interface declares creation methods for each distinct
 * product type.
 */
interface TemplateFactory
{
    public function createTitleTemplate(): TitleTemplate;

    public function createPageTemplate(): PageTemplate;

    public function getRenderer(): TemplateRenderer;
}

/**
 * Each Concrete Factory corresponds to a specific variant (or family) of
 * products.
 *
 * This Concrete Factory creates Twig templates.
 */
class TwigTemplateFactory implements TemplateFactory
{
    public function createTitleTemplate(): TitleTemplate
    {
        return new TwigTitleTemplate();
    }

    public function createPageTemplate(): PageTemplate
    {
        return new TwigPageTemplate($this->createTitleTemplate());
    }

    public function getRenderer(): TemplateRenderer
    {
        return new TwigRenderer();
    }
}

/**
 * And this Concrete Factory creates PHPTemplate templates.
 */
class PHPTemplateFactory implements TemplateFactory
{
    public function createTitleTemplate(): TitleTemplate
    {
        return new PHPTemplateTitleTemplate();
    }

    public function createPageTemplate(): PageTemplate
    {
        return new PHPTemplatePageTemplate($this->createTitleTemplate());
    }

    public function getRenderer(): TemplateRenderer
    {
        return new PHPTemplateRenderer();
    }
}

/**
 * Each distinct product type should have a separate interface. All variants of
 * the product must follow the same interface.
 *
 * For instance, this Abstract Product interface describes the behavior of page
 * title templates.
 */
interface TitleTemplate
{
    public function getTemplateString(): string;
}

/**
 * This Concrete Product provides Twig page title templates.
 */
class TwigTitleTemplate implements TitleTemplate
{
    public function getTemplateString(): string
    {
        return "<h1>{{ title }}</h1>";
    }
}

/**
 * And this Concrete Product provides PHPTemplate page title templates.
 */
class PHPTemplateTitleTemplate implements TitleTemplate
{
    public function getTemplateString(): string
    {
        return "<h1><?= \$title; ?></h1>";
    }
}

/**
 * This is another Abstract Product type, which describes whole page templates.
 */
interface PageTemplate
{
    public function getTemplateString(): string;
}

/**
 * The page template uses the title sub-template, so we have to provide the way
 * to set it in the sub-template object. The abstract factory will link the page
 * template with a title template of the same variant.
 */
abstract class BasePageTemplate implements PageTemplate
{
    protected $titleTemplate;

    public function __construct(TitleTemplate $titleTemplate)
    {
        $this->titleTemplate = $titleTemplate;
    }
}

/**
 * The Twig variant of the whole page templates.
 */
class TwigPageTemplate extends BasePageTemplate
{
    public function getTemplateString(): string
    {
        $renderedTitle = $this->titleTemplate->getTemplateString();

        return <<<HTML
        <div class="page">
            $renderedTitle
            <article class="content">{{ content }}</article>
        </div>
        HTML;
    }
}

/**
 * The PHPTemplate variant of the whole page templates.
 */
class PHPTemplatePageTemplate extends BasePageTemplate
{
    public function getTemplateString(): string
    {
        $renderedTitle = $this->titleTemplate->getTemplateString();

        return <<<HTML
        <div class="page">
            $renderedTitle
            <article class="content"><?= \$content; ?></article>
        </div>
        HTML;
    }
}

/**
 * The renderer is responsible for converting a template string into the actual
 * HTML code. Each renderer behaves differently and expects its own type of
 * template strings passed to it. Baking templates with the factory let you pass
 * proper types of templates to proper renders.
 */
interface TemplateRenderer
{
    public function render(string $templateString, array $arguments = []): string;
}

/**
 * The renderer for Twig templates.
 */
class TwigRenderer implements TemplateRenderer
{
    public function render(string $templateString, array $arguments = []): string
    {
        return \Twig::render($templateString, $arguments);
    }
}

/**
 * The renderer for PHPTemplate templates. Note that this implementation is very
 * basic, if not crude. Using the `eval` function has many security
 * implications, so use it with caution in real projects.
 */
class PHPTemplateRenderer implements TemplateRenderer
{
    public function render(string $templateString, array $arguments = []): string
    {
        extract($arguments);

        ob_start();
        eval(' ?>' . $templateString . '<?php ');
        $result = ob_get_contents();
        ob_end_clean();

        return $result;
    }
}

/**
 * The client code. Note that it accepts the Abstract Factory class as the
 * parameter, which allows the client to work with any concrete factory type.
 */
class Page
{

    public $title;

    public $content;

    public function __construct($title, $content)
    {
        $this->title = $title;
        $this->content = $content;
    }

    // Here's how would you use the template further in real life. Note that the
    // page class does not depend on any concrete template classes.
    public function render(TemplateFactory $factory): string
    {
        $pageTemplate = $factory->createPageTemplate();

        $renderer = $factory->getRenderer();

        return $renderer->render($pageTemplate->getTemplateString(), [
            'title' => $this->title,
            'content' => $this->content
        ]);
    }
}

/**
 * Now, in other parts of the app, the client code can accept factory objects of
 * any type.
 */
$page = new Page('Sample page', 'This is the body.');

echo "Testing actual rendering with the PHPTemplate factory:\n";
echo $page->render(new PHPTemplateFactory());


// Uncomment the following if you have Twig installed.

// echo "Testing rendering with the Twig factory:\n"; echo $page->render(new
// TwigTemplateFactory());
```
