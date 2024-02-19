---
layout: post
img: ../assets/imgs/creational-design-patterns.jpg
summary: Creational design patterns offer proven techniques for managing the creation of objects in software development, leading to more efficient and effective code that is scalable and maintainable. This article provides an overview of the Abstract Factory, Factory Method, Singleton, Builder, and Prototype patterns, along with TypeScript examples.
---

##### Introduction

In software development, creational design patterns offer a set of proven techniques and principles for managing the creation of objects. Object creation can be complex, prone to design problems and increased code complexity. Creational design patterns provide mechanisms that empower developers to create objects in a more controlled, flexible, and reusable manner. By effectively managing object creation, these patterns enable the development of scalable and maintainable code that can be easily modified and extended over time.

Several widely recognized creational design patterns include the Singleton, Factory Method, and Abstract Factory patterns. Each pattern presents a distinct approach to object creation, providing developers with various options to achieve specific goals. For instance, the Singleton pattern ensures the existence of only one instance of a particular object, which proves useful when dealing with resources that should not be duplicated. The Factory Method pattern enables object creation without specifying their exact type, promoting flexibility and decoupling between creators and created objects. The Abstract Factory pattern establishes an interface for creating families of related objects, facilitating the creation of multiple object types that collaborate seamlessly.

By leveraging creational design patterns, developers can enhance their software's robustness, flexibility, and maintainability, irrespective of the project's specific requirements. These patterns enable the creation of more efficient and effective code, leading to improved performance and heightened user satisfaction. Incorporating creational design patterns becomes an essential aspect of software development, significantly elevating the quality and functionality of the final product.

##### Abstract Factory

The **_Abstract Factory_** pattern provides a robust methodology for constructing families of related or dependent objects, circumventing the need to denote their precise classes explicitly. It permits a system to operate smoothly with any concrete classes following the abstract factory's blueprint. The pattern streamlines the interaction between client code and the system, eliminating clients' need to concern themselves with individual class creation. Additionally, it grants the system independence from the specifics of how its products are created, composed, and represented.

The diagram below illustrates an implementation of the Abstract Factory pattern. The depicted system contains two related object families: _FileOperation_ and _ProcessOperation_. Each family exhibits two concrete implementations: _Windows_ and _MacOS_. Utilizing the `OperatingSystemOperationFactory`, the system fabricates the appropriate object family contingent on the operating system in operation. The _SystemOperationManager_ class serves as the client, employing the factory to generate the necessary objects and initiate their operation.

```ts
// --------------------------------------------------------------------------
// ------------------------ Abstract Factory Pattern ------------------------
// --------------------------------------------------------------------------

/**
 * FileOperation interface defines methods for file operations.
 */
interface FileOperation {
  readFile(path: string[]): void;
  writeFile(path: string[], content: string): void;
  deleteFile(path: string[]): void;
}

/**
 * ProcessOperation interface defines methods for process operations.
 */
interface ProcessOperation {
  startProcess(name: string): void;
  killProcess(name: string): void;
}

/**
 * WindowsFileOperation class implements the FileOperation interface for Windows OS.
 */
class WindowsFileOperation implements FileOperation {
  public readFile(path: string[]): void {
    console.log(`Reading file at C:${path.join("/")} on Windows.`);
  }

  public writeFile(path: string[], content: string): void {
    console.log(
      `Writing file at C:${path.join("/")} with content ${content} on Windows.`
    );
  }

  public deleteFile(path: string[]): void {
    console.log(`Deleting file at C:${path.join("/")} on Windows.`);
  }
}

/**
 * WindowsProcessOperation class implements the ProcessOperation interface for Windows OS.
 */
class WindowsProcessOperation implements ProcessOperation {
  public startProcess(name: string): void {
    console.log(`Starting process ${name} on Windows.`);
  }

  public killProcess(name: string): void {
    console.log(`Killing process ${name} on Windows.`);
  }
}

/**
 * MacOSFileOperation class implements the FileOperation interface for MacOS.
 */
class MacOSFileOperation implements FileOperation {
  public readFile(path: string[]): void {
    console.log(`Reading file at ${path.join("\\")} on MacOS.`);
  }

  public writeFile(path: string[], content: string): void {
    console.log(
      `Writing file at ${path.join("\\")} with content ${content} on MacOS.`
    );
  }

  public deleteFile(path: string[]): void {
    console.log(`Deleting file at ${path.join("\\")} on MacOS.`);
  }
}

/**
 * MacOSProcessOperation class implements the ProcessOperation interface for MacOS.
 */
class MacOSProcessOperation implements ProcessOperation {
  public startProcess(name: string): void {
    console.log(`Starting process ${name} on MacOS.`);
  }

  public killProcess(name: string): void {
    console.log(`Killing process ${name} on MacOS.`);
  }
}

/**
 * OperatingSystemOperationFactory is an abstract factory interface
 * for creating instances of FileOperation and ProcessOperation interfaces.
 */
interface OperatingSystemOperationFactory {
  createFileOperation(): FileOperation;
  createProcessOperation(): ProcessOperation;
}

/**
 * WindowsOperationsFactory is a concrete factory class for Windows OS.
 * It implements the OperatingSystemOperationFactory interface.
 */
class WindowsOperationsFactory implements OperatingSystemOperationFactory {
  public createFileOperation(): FileOperation {
    return new WindowsFileOperation();
  }

  public createProcessOperation(): ProcessOperation {
    return new WindowsProcessOperation();
  }
}

/**
 * MacOperationsFactory is a concrete factory class for MacOS.
 * It implements the OperatingSystemOperationFactory interface.
 */
class MacOperationsFactory implements OperatingSystemOperationFactory {
  public createFileOperation(): FileOperation {
    return new MacOSFileOperation();
  }

  public createProcessOperation(): ProcessOperation {
    return new MacOSProcessOperation();
  }
}

/**
 * Client class is independent of the concrete classes.
 * It uses the abstract factory and interfaces to perform operations.
 */
class SystemOperationManager {
  private fileOperation: FileOperation;
  private processOperation: ProcessOperation;

  constructor(factory: OperatingSystemOperationFactory) {
    this.fileOperation = factory.createFileOperation();
    this.processOperation = factory.createProcessOperation();
  }

  public run(): void {
    this.fileOperation.readFile(["Users", "Documents", "file.txt"]);
    this.fileOperation.writeFile(
      ["Users", "Documents", "file.txt"],
      "Hello World!"
    );
    this.fileOperation.deleteFile(["Users", "Documents", "file.txt"]);

    this.processOperation.startProcess("Chrome");
    this.processOperation.killProcess("Chrome");
  }
}
```

##### Factory Method

The **_Factory Method_** pattern empowers subclasses to modify the type of objects a superclass creates. This pattern prescribes an interface for constructing objects and proposes utilizing a dedicated factory method instead of direct object construction calls. Even though objects continue to be instantiated using the new operator, this operation is executed within the confines of the factory method, the _creator_ in the pattern's terminology. The products of the factory method are typically known as _products_.

The ensuing diagram exemplifies the Factory Method pattern. It includes an abstract **_[Page](./code/page-ui-factory.ts)_** class with a factory method _createTheme()_ that subclasses override to provide their versions of the UITheme interface. The UITheme interface has two concrete implementations: _DarkTheme_ and _LightTheme_. Each theme specifies its primary and secondary colors based on the _ThemeType_ enum. The _UIThemeRenderer_, acting as the client, uses the Page factory to create and render the appropriate theme.

```ts
// --------------------------------------------------------------------------
// ------------------------- Factory Method Pattern -------------------------
// --------------------------------------------------------------------------

/**
 * The ThemeType Enum defines the possible types of themes.
 */
enum ThemeType {
  DARK = "#000000",
  LIGHT = "#FFFFFF",
}

/**
 * The UITheme (Product) interface declares the properties of the UI theme.
 * It defines the primary and secondary colors of the theme.
 */
interface UITheme {
  get primaryColor(): ThemeType;
  get secondaryColor(): ThemeType;
}

/**
 * Concrete UITheme (Product) classes provide various implementations of the UITheme interface for different types of themes.
 * The DarkTheme class defines the properties of a dark theme (primary color is black, secondary color is white).
 * The LightTheme class defines the properties of a light theme (primary color is white, secondary color is black).
 */
class DarkTheme implements UITheme {
  public get primaryColor(): ThemeType {
    return ThemeType.DARK;
  }

  public get secondaryColor(): ThemeType {
    return ThemeType.LIGHT;
  }
}

class LightTheme implements UITheme {
  public get primaryColor(): ThemeType {
    return ThemeType.LIGHT;
  }

  public get secondaryColor(): ThemeType {
    return ThemeType.DARK;
  }
}

/**
 * The Page (Creator) abstract class declares the factory method that is supposed to return an object of a UITheme (Product) class.
 */
abstract class Page {
  public abstract createTheme(): UITheme;

  public render(): string {
    const theme = this.createTheme();
    return `<body style="background-color: ${theme.primaryColor}; color: ${theme.secondaryColor};"> <h1>Factory Method</h1> </body>`;
  }
}

/**
 * Concrete Page (Creator) classes override the factory method to change the resulting UITheme (Product) class.
 * The DarkPage class returns a DarkTheme (Product) class.
 * The LightPage class returns a LightTheme (Product) class.
 */
class DarkPage extends Page {
  public createTheme(): UITheme {
    return new DarkTheme();
  }
}

class LightPage extends Page {
  public createTheme(): UITheme {
    return new LightTheme();
  }
}

class DynamicPage extends Page {
  private isDayTime: boolean;
  private now: Date = new Date();

  constructor() {
    super();
    this.isDayTime = this.now.getHours() >= 6 && this.now.getHours() <= 18;
  }

  public createTheme(): UITheme {
    return this.isDayTime ? new LightTheme() : new DarkTheme();
  }
}

/**
 * The Client class uses the factory method to create a UITheme (Product) class.
 * The main method calls the render method of the Page (Creator) class.
 */
class UIThemeRenderer {
  private pageFactory: Page;

  constructor(pageFactory: Page) {
    this.pageFactory = pageFactory;
  }

  public render(): string {
    return this.pageFactory.render();
  }
}
```

##### Singleton

The **_Singleton_** pattern is a design paradigm that assures a class has only one instance and furnishes a universal access point to this instance. By ensuring that a class has exactly one instance and providing a global access point to it, this pattern simultaneously resolves two problems. It is effective in abiding by the _Single Responsibility Principle_.

Here is a diagram representing an implementation of the Singleton pattern. The **_[LogMonitor](./code/log-monitor-singleton.ts)_** class is the singleton, meaning only one instance of LogMonitor can exist. It maintains a collection of logs. The _getInstance()_ method ensures only one instance of LogMonitor is created. Clients interact with the LogMonitor singleton through the _LogController_ class.

```ts
// ---------------------------------------------------------------------------
// --------------------------- Singleton Design Pattern ----------------------
// ---------------------------------------------------------------------------

/**
 * LogMessage is an enumeration that outlines different types of log statuses.
 * OK: Log status indicating successful operation.
 * WARNING: Log status indicating a potential issue.
 * ERROR: Log status indicating a failure or error in operation.
 */
enum LogMessage {
  OK,
  WARNING,
  ERROR,
}

/**
 * The Log class is responsible for creating log messages. Each log has a message, unique code and a timestamp.
 */
class Log {
  private message: LogMessage;
  private code: Symbol;
  private timestamp: Date;

  public constructor(message: LogMessage) {
    this.message = message;
    this.code = Symbol();
    this.timestamp = new Date();
  }

  public getMessage(): LogMessage {
    return this.message;
  }

  public getCode(): Symbol {
    return this.code;
  }

  public getTimestamp(): Date {
    return this.timestamp;
  }
}

/**
 * The LogMonitor class is a Singleton class that tracks and stores logs.
 * It restricts its instantiation to a single instance and provides a global point of access to it.
 * It includes functionality to add logs and retrieve the complete list of logs.
 */
class LogMonitor {
  private static instance: LogMonitor;
  private logs: Log[];

  private constructor() {
    this.logs = [];
  }

  public log(log: Log) {
    this.logs.push(log);
  }

  public getLogs(): Log[] {
    return this.logs;
  }

  public static getInstance(): LogMonitor {
    if (!LogMonitor.instance) {
      LogMonitor.instance = new LogMonitor();
    }

    return LogMonitor.instance;
  }
}

/**
 * The LogController class is the client that interacts with the LogMonitor singleton.
 * It utilizes the LogMonitor's functionality to add and retrieve logs.
 */
class LogController {
  private logMonitor: LogMonitor;

  public constructor() {
    this.logMonitor = LogMonitor.getInstance();
  }

  public log(log: Log): void {
    this.logMonitor.log(log);
  }

  public getLogs(): Log[] {
    return this.logMonitor.getLogs();
  }
}
```

##### Builder

The **_Builder_** pattern is a creative approach to constructing complex objects step-by-step. This pattern shields clients from the intricacies of the construction process, allowing them to generate a complex object by defining its type and content only. The construction process is separated from the representation, allowing varied representations to be created using the same building process.

Below is a diagram demonstrating an example of the Builder pattern. The **_[HtmlBuilder](./code/html-form-builder.ts)_** is the abstract builder that defines the steps to build an HTML form. _FormBuilder_ is a concrete builder that implements the steps defined in HtmlBuilder. _Director_ uses the FormBuilder to construct specific forms such as contact and login forms. _HtmlFormComposer_ is the client working with the Director to construct the forms.

```ts
// --------------------------------------------------------------------------
// --------------------------- Template Pattern -----------------------------
// --------------------------------------------------------------------------

/**
 * The Product class defines the elements and attributes of the HTML form.
 * It also defines the methods to render the HTML form and to get the HTML.
 */
class HtmlTemplate {
  private elements: string[];

  public constructor() {
    this.elements = [];
  }

  public get size(): number {
    return this.elements.length;
  }

  public get firstElement(): string {
    return this.elements[0];
  }

  public get lastElement(): string {
    return this.elements[this.size - 1];
  }

  public set lastElement(element: string) {
    this.elements[this.size - 1] = element;
  }

  public addElement(element: string) {
    this.elements.push(element);
  }

  public getElements(): string[] {
    return this.elements;
  }

  public render() {
    return `<div>\n ${this.elements.join("\n")} \n</div>`;
  }
}

/**
 * HtmlElement interface defines the structure of an HTML element.
 * It defines the tag, content and type of the element.
 * The type is only used for input elements.
 */
interface HtmlElement {
  tag: string;
  content: string;
  type?: string;
}

/**
 * ElementAttribute interface defines the structure of an HTML element attribute.
 * It defines the name and value of the attribute.
 */
interface ElementAttribute {
  name: string;
  value: string;
}

/**
 * ElementStyle interface defines the structure of an HTML element style.
 * It defines the property and value of the style.
 */
interface ElementStyle {
  property: string;
  value: string;
}

/**
 * HtmlBuilder is the abstract builder class.
 * It defines the methods to add elements, attributes and styles.
 */
abstract class HtmlBuilder {
  protected html: HtmlTemplate;

  public constructor() {
    this.reset();
  }

  public reset(): void {
    this.html = new HtmlTemplate();
  }

  public abstract addElement(element: HtmlElement): HtmlBuilder;
  public abstract addAttribute(attribute: ElementAttribute): HtmlBuilder;
  public abstract addStyle(style: ElementStyle): HtmlBuilder;
  public abstract build(): string;
}

/**
 * FormBuilder is the concrete builder class.
 * It overrides the methods to add elements, attributes, styles and to build the HTML.
 */
class FormBuilder extends HtmlBuilder {
  public constructor() {
    super();
  }

  public override addElement(element: HtmlElement): FormBuilder {
    switch (element.tag) {
      case "input":
        this.html.addElement(`<div>\n<input type="${element.type}">\n</div>`);
        return this;
      case "textarea":
        this.html.addElement(
          `<div>\n<textarea>${element.content}</textarea>\n</div>`
        );
        return this;
      default:
        this.html.addElement(
          `<${element.tag}>${element.content}</${element.tag}>`
        );
        return this;
    }
  }

  public override addAttribute(attribute: ElementAttribute): FormBuilder {
    this.html.getElements()[this.html.size - 1] = this.html.lastElement.replace(
      ">",
      ` ${attribute.name}="${attribute.value}">`
    );
    return this;
  }

  public override addStyle(style: ElementStyle): FormBuilder {
    const lastIndex = this.html.size - 1;

    if (this.html.lastElement.includes("style=")) {
      this.html.lastElement = this.html.lastElement.replace(
        'style="',
        `style="${style.property}: ${style.value}; `
      );
    } else {
      this.html.lastElement = this.html.lastElement.replace(
        ">",
        ` style="${style.property}: ${style.value};">`
      );
    }

    this.html.getElements()[lastIndex] = this.html.lastElement;
    return this;
  }

  public override build(): string {
    return `<form>\n ${this.html.render()} \n</form>`;
  }
}

/**
 * The Director class defines the methods to build the HTML form.
 * It uses the builder to add elements, attributes and styles.
 */
class Director {
  private builder: FormBuilder;

  public constructor(builder: FormBuilder) {
    this.builder = builder;
  }

  public buildContactForm(): string {
    this.builder.reset();
    this.builder
      .addElement({ tag: "h1", content: "Contact Form" })
      .addElement({ tag: "p", content: "Please enter your details" })
      .addElement({ tag: "label", content: "Name" })
      .addAttribute({ name: "for", value: "name" })
      .addElement({ tag: "input", content: "", type: "text" })
      .addAttribute({ name: "id", value: "name" })
      .addAttribute({ name: "name", value: "name" })
      .addAttribute({ name: "placeholder", value: "Enter your name" })
      .addElement({ tag: "label", content: "Email" })
      .addAttribute({ name: "for", value: "email" })
      .addElement({ tag: "input", content: "", type: "email" })
      .addAttribute({ name: "id", value: "email" })
      .addAttribute({ name: "name", value: "email" })
      .addAttribute({ name: "placeholder", value: "Enter your email" })
      .addElement({ tag: "label", content: "Message" })
      .addAttribute({ name: "for", value: "message" })
      .addElement({ tag: "textarea", content: "" })
      .addAttribute({ name: "id", value: "message" })
      .addAttribute({ name: "name", value: "message" })
      .addAttribute({ name: "placeholder", value: "Enter your message" });
    return this.builder.build();
  }

  public buildLoginForm(): string {
    this.builder.reset();
    this.builder
      .addElement({ tag: "h1", content: "Login Form" })
      .addElement({ tag: "p", content: "Please enter your credentials" })
      .addElement({ tag: "label", content: "Username" })
      .addAttribute({ name: "for", value: "username" })
      .addElement({ tag: "input", content: "", type: "text" })
      .addAttribute({ name: "id", value: "username" })
      .addAttribute({ name: "name", value: "username" })
      .addAttribute({ name: "placeholder", value: "Enter your username" })
      .addElement({ tag: "label", content: "Password" })
      .addAttribute({ name: "for", value: "password" })
      .addElement({ tag: "input", content: "", type: "password" })
      .addAttribute({ name: "id", value: "password" })
      .addAttribute({ name: "name", value: "password" })
      .addAttribute({ name: "placeholder", value: "Enter your password" });
    return this.builder.build();
  }
}

/**
 * Client class defines the methods to build the HTML form.
 * It uses the director to build the form.
 */
class HtmlFormComposer {
  private director: Director;
  private builder: FormBuilder;

  public constructor() {
    this.builder = new FormBuilder();
    this.director = new Director(this.builder);
  }

  public buildContactForm(): string {
    return this.director.buildContactForm();
  }

  public buildLoginForm(): string {
    return this.director.buildLoginForm();
  }
}
```

##### Prototype

The **_Prototype_** pattern is a design pattern that facilitates the cloning of objects, independent of their specific classes. This pattern delegates the responsibility of cloning to the objects by introducing a common interface for all clonable objects. The reliance on a standard interface ensures that the cloning operation is class-agnostic.

An illustrative diagram shows a system using the Prototype pattern to clone objects representing catalog items. The **_[Catalog](./code/catalog-prototype.ts)_** is the prototype capable of clone itself. There are two concrete types of catalogs: _ProductCatalog_ and _ServiceCatalog_. The _CatalogRegistry_ is used to store the catalogs. A _CatalogManager_, acting as the client, can add new catalogs to the registry and clone existing ones.

```ts
// --------------------------------------------------------------------------
// --------------------------- Prototype Pattern ----------------------------
// --------------------------------------------------------------------------

/**
 * The Year type is a string literal type that represents a year.
 */
type Year = `${number}${number}${number}${number}`;

/**
 * The CatalogItem interface represents an item in a catalog.
 * The name property is a string that represents the name of the item.
 * The price property is a number that represents the price of the item.
 */
interface CatalogItem {
  name: string;
  price: number;
}

/**
 * The Catalog abstract class declares the clone method that returns a clone of the Catalog object.
 */
abstract class Catalog {
  public year: Year;
  protected items: CatalogItem[];

  public constructor(self?: Catalog) {
    if (self) {
      this.year = self.year;
      this.items = self.items;
    } else {
      this.items = [];
    }
  }

  public addItem(item: CatalogItem): void {
    this.items.push(item);
  }

  public abstract clone(): Catalog;
}

/**
 * Concrete Catalog sub-classes override the clone method to return a clone of the specific Catalog sub-class object.
 * The ProductCatalog class returns a clone of the ProductCatalog object.
 * The ServiceCatalog class returns a clone of the ServiceCatalog object.
 */
class ProductCatalog extends Catalog {
  public constructor(self?: ProductCatalog) {
    if (self) {
      super(self);
    } else {
      super();
    }
  }

  public override clone(): Catalog {
    return new ProductCatalog(this);
  }
}

class ServiceCatalog extends Catalog {
  public constructor(self?: ServiceCatalog) {
    if (self) {
      super(self);
    } else {
      super();
    }
  }

  public override clone(): Catalog {
    return new ServiceCatalog(this);
  }
}

/**
 * The CatalogRegistry class stores Catalog objects and provides a method to get a clone of a Catalog object.
 * The add method adds a Catalog object to the registry.
 * The get method returns a clone of a Catalog object from the registry.
 */
class CatalogRegistry {
  public constructor(
    private readonly catalogs: Map<Year, Catalog> = new Map<Year, Catalog>()
  ) {}

  public add(catalog: Catalog): void {
    this.catalogs.set(catalog.year, catalog);
  }

  public get(year: Year): Catalog {
    if (!this.catalogs.has(year)) {
      throw new Error(`Catalog for year ${year} not found`);
    }

    return this.catalogs.get(year)!.clone();
  }
}

/**
 * The CatalogManager class creates a CatalogRegistry object and adds Catalog objects to the registry.
 * The addProductCatalog method adds a ProductCatalog object to the registry.
 * The addServiceCatalog method adds a ServiceCatalog object to the registry.
 * The cloneProductCatalog method returns a clone of a ProductCatalog object from the registry.
 * The cloneServiceCatalog method returns a clone of a ServiceCatalog object from the registry.
 */
class CatalogManager {
  private registry: CatalogRegistry;

  public constructor() {
    this.registry = new CatalogRegistry();
  }

  public addProductCatalog(catalog: ProductCatalog): void {
    this.registry.add(catalog);
  }

  public addServiceCatalog(catalog: ServiceCatalog): void {
    this.registry.add(catalog);
  }

  public cloneProductCatalog(year: Year): Catalog {
    return this.registry.get(year);
  }

  public cloneServiceCatalog(year: Year): Catalog {
    return this.registry.get(year);
  }
}
```
