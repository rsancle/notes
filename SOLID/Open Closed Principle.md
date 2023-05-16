# Open Closed Principe (OCP)
The aim of OPC is making classes **open to extension** but **closed to modification**.

## What does it mean?
It means that classes shouldn't be modified and they should be built allowing them to be extended in order to add new functionallities.

## How to apply it?
We could use different patterns to apply it but the key is developing a class thinking that it won't be modificated. 
We could add new functionalities extending the class applying polymorfism for example.

## Code example
```
enum Color
{
  RED, GREEN, BLUE
}

enum Size
{
  SMALL, MEDIUM, LARGE, YUGE
}

class Product
{
  public String name;
  public Color color;
  public Size size;

  public Product(String name, Color color, Size size) {
    this.name = name;
    this.color = color;
    this.size = size;
  }
}

class ProductFilter
{
  public Stream<Product> filterByColor(List<Product> products, Color color)
  {
    return products.stream().filter(p -> p.color == color);
  }

  public Stream<Product> filterBySize(List<Product> products, Size size)
  {
    return products.stream().filter(p -> p.size == size);
  }

  public Stream<Product> filterBySizeAndColor(List<Product> products, Size size, Color color)
  {
    return products.stream().filter(p -> p.size == size && p.color == color);
  }
  // state space explosion
  // 3 criteria = 7 methods
}

// we introduce two new interfaces that are open for extension
interface Specification<T>
{
  boolean isSatisfied(T item);
}

interface Filter<T>
{
  Stream<T> filter(List<T> items, Specification<T> spec);
}

class ColorSpecification implements Specification<Product>
{
  private Color color;

  public ColorSpecification(Color color) {
    this.color = color;
  }

  @Override
  public boolean isSatisfied(Product p) {
    return p.color == color;
  }
}

class SizeSpecification implements Specification<Product>
{
  private Size size;

  public SizeSpecification(Size size) {
    this.size = size;
  }

  @Override
  public boolean isSatisfied(Product p) {
    return p.size == size;
  }
}

class AndSpecification<T> implements Specification<T>
{
  private Specification<T> first, second;

  public AndSpecification(Specification<T> first, Specification<T> second) {
    this.first = first;
    this.second = second;
  }

  @Override
  public boolean isSatisfied(T item) {
    return first.isSatisfied(item) && second.isSatisfied(item);
  }

}

class BetterFilter implements Filter<Product>
{
  @Override
  public Stream<Product> filter(List<Product> items, Specification<Product> spec) {
    return items.stream().filter(p -> spec.isSatisfied(p));
  }
}

class OCPDemo
{
  public static void main(String[] args) {
    Product apple = new Product("Apple", Color.GREEN, Size.SMALL);
    Product tree = new Product("Tree", Color.GREEN, Size.LARGE);
    Product house = new Product("House", Color.BLUE, Size.LARGE);

    List<Product> products = List.of(apple, tree, house);

    ProductFilter pf = new ProductFilter();
    System.out.println("Green products (old):");
    pf.filterByColor(products, Color.GREEN)
      .forEach(p -> System.out.println(" - " + p.name + " is green"));

    // ^^ BEFORE

    // vv AFTER
    BetterFilter bf = new BetterFilter();
    System.out.println("Green products (new):");
    bf.filter(products, new ColorSpecification(Color.GREEN))
      .forEach(p -> System.out.println(" - " + p.name + " is green"));

    System.out.println("Large products:");
    bf.filter(products, new SizeSpecification(Size.LARGE))
      .forEach(p -> System.out.println(" - " + p.name + " is large"));

    System.out.println("Large blue items:");
    bf.filter(products,
      new AndSpecification<>(
        new ColorSpecification(Color.BLUE),
        new SizeSpecification(Size.LARGE)
      ))
      .forEach(p -> System.out.println(" - " + p.name + " is large and blue"));
  }
}
```

## Another example
In this case it is easer to understand. The following class uses a parser builder to parse an item. We know how it has to be made but not how it will be implemented. So we create an abstarct class with the logic of how elements will be parsed, but with an abstract builder method that will handle the parse action depending on the implementation. We shouldn't modify the abstract class so it's closed, but it is opened to its extension implementing new classes.
 ```
 abstract class FeedStockCounter {
    totalStock(feed: Feed): number {
        const flowers = this.buildParser().parse(feed);
        return this.countTotalStock(flowers);
    }

    private countTotalStock(flowers: Flower[]): number {
        return flowers.reduce((acc, flower) => {
            return acc + flower.totalStock();
        }, 0);
    }

    protected abstract buildParser(): FeedParser;
}

class FeedStockCounterCsv extends FeedStockCounter {
    protected buildParser() {
        return new FeedParserCsv();
    }
}
```
