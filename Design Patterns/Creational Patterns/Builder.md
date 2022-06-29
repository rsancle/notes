# Builder Pattern
It solves the problem of creating objects with a lot of arguments.

## Example
Problem code
```
// Let's make a vacation plan

Planner planner = new Planner(
  hotels = ["Big Hotel"],
  tickets = [...],
  days = [...]
);

class Planner {
  constructor(array hotels, array tickets, array days)
}
```

Solution
```
// Let's make a vacation plan

PlannerBuilderInterface plannerBuilder = new PlannerBuilder();
plannerBuilder.buildDay(date);
plannerBuilder.addHotel("Gran Hotel");
plannerBuilder.addTicket("Zoo");
Planner planner = plannerBuilder.getPlanner();


interface PlannerBuilderInterface() {
  public void buildDay(Date date);
  public void addHotel(String hotelName);
  public void addTicket(String ticketName);
  public Planner getPlanner();
}

```

## Fluent builder Example
The code should be quite similar but it allows to us to chain different actions

```
// Let's make a vacation plan

Planner planner = new PlannerBuilder();
  .buildDay(date);
  .addHotel("Gran Hotel");
  .addTicket("Zoo");
  .getPlanner();



interface PlannerBuilderInterface() {
  public PlannerBuilderInterface buildDay(Date date);
  public PlannerBuilderInterface addHotel(String hotelName);
  public PlannerBuilderInterface addTicket(String ticketName);
  public Planner getPlanner();
}

```
