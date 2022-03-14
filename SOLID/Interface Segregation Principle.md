# Interface Segregation Principle (ISP)
The Interface Segregation Principle (ISP) approaches the problem of interfaces with many methods for several clients, many of which do not use all the methods.
# What does it mean?
Segregating an Interface in a smaller interfaces will give us more flexibility, otherwise, if we have a big interface it will force to us to implement methods that we don't need to use.

```
class Document
{
}

interface Machine
{
  void print(Document d);
  void fax(Document d) throws Exception;
  void scan(Document d) throws Exception;
}

// ok if you need a multifunction machine
class MultiFunctionPrinter implements Machine
{
  public void print(Document d)
  {
    //
  }

  public void fax(Document d)
  {
    //
  }

  public void scan(Document d)
  {
    //
  }
}

class OldFashionedPrinter implements Machine
{
  public void print(Document d)
  {
    // yep
  }

  public void fax(Document d) throws Exception
  {
    throw new Exception();
  }

  public void scan(Document d) throws Exception
  {
    throw new Exception();
  }
}

interface Printer
{
  void Print(Document d) throws Exception;
}

interface IScanner
{
  void Scan(Document d) throws Exception;
}

class JustAPrinter implements Printer
{
  public void Print(Document d)
  {

  }
}

class Photocopier implements Printer, IScanner
{
  public void Print(Document d) throws Exception
  {
    throw new Exception();
  }

  public void Scan(Document d) throws Exception
  {
    throw new Exception();
  }
}

interface MultiFunctionDevice extends Printer, IScanner //
{

}

class MultiFunctionMachine implements MultiFunctionDevice
{
  // compose this out of several modules
  private Printer printer;
  private IScanner scanner;

  public MultiFunctionMachine(Printer printer, IScanner scanner)
  {
    this.printer = printer;
    this.scanner = scanner;
  }

  public void Print(Document d) throws Exception
  {
    printer.Print(d);
  }

  public void Scan(Document d) throws Exception
  {
    scanner.Scan(d);
  }
}

