# Dependency Inversion Principle (DIP)
It says that: 
- High-level modules should not depend on low-level modules, both should depend on abstactions.
- Abstractions should not depend on details. Details should depend on abstractions.

# What does it mean?
Bascally it says that a class shouldn't recieve an implementation class as a dependency, it should recieve a Interface instead.
## Concepts
- Abstaction: Interface or Abstract class
- Details: Implemented Class
- High-level module: Class that has a database class dependency
- Low-level module: Class that implements the database dependency details, for example MySQLDB and MongoDB

# Code example
Wrong code:
```
class CreatorClass{
    private MySQLDB database;
    
    public void CreatorClass(MySQLDB database)
    {
      this.database = database;
    };
    
    public void create(array data)
    {
      this.database.insert(data);
    }
}
```
Good code:
```
class CreatorClass{
    private DBInterface database;
    
    public void CreatorClass(DBInterface database)
    {
      this.database = database;
    };
    
    public void create(array data)
    {
      this.database.insert(data);
    }
}

class MySQLDB implements DBInterface {}
class MongoDB implements DBInterface {}

```
# Exceptions
Not always is needed to inject interfaces. For example, the controller could recieve an implementation since it includes the bussiness logic. Business logic may change since we decide how it must work. DIP must be applied usually in I/O actions, since it could need a change depending on database versions for example.

## References
-https://dev.to/tamerlang/understanding-solid-principles-dependency-inversion-1b0f
