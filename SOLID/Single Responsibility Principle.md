# Singe Resposibility Principle (SRP)
That principle say that a class must have just one reposibility.
## What does it mean?
It means that, for example, a class should not mix the business logic with getting/storing data.
## How to solve it?
Applying the SRP, we would create different classes for example:
- The CustomerService: Its reposibility is how customer data must be handled
- The CustomerRepository: Its reponsibility is to make the needed operations to store/get data from our database
## What is the problem?
Sometimes is difficult to define the responsibility limit. For example we said that the CustomerService will include the operations to handle the customer data.

Adding all the operations related to de customer could not break at all the SRP but it could make our class difficult to understand or follow if it has a lot of methods.

To avoid so we have to have in mind breaking up the class in smaller classes in order to have smaller responsibilities
