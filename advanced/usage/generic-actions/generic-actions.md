---
description: Radical Simplification with Reusable Action Libraries
---

# Reusable Generic Actions

NgRx Auto-Entity provides a relatively simple solution to the _**action triplet trifurcation conundrum**_. Make commonly-implemented actions reusable! If there ever was a use case for generics, CRUD entity actions would seem to be as sublimely perfect a case as ever possible. By making actions **generic**, this allows a simple library of standard CRUD actions to be implemented that could then be easily reused by any number of unique entities. 

### Actions Made For You

Generic actions are the primary interface with which an application interacts with NgRx Auto-Entity. No need to implement the same pattern of three actions again and again for each and every entity! We have provided all the necessary actions for you. You simply need to dispatch them! 

{% code-tabs %}
{% code-tabs-item title="customers.component.ts" %}
```typescript
import {Create, LoadAll} from '@briebug/ngrx-auto-entity';
import {Customer} from 'models';
import {allCustomers} from 'state/customer.state';

@Component(...)
export class CustomersComponent implements OnInit {
    customers$: Observable<Customer[]>;
    
    constructor(
        private activatedRoute: ActivatedRoute, 
        private store: Store<AppState>
    ) {}
    
    ngOnInit() {
        this.customer$ = this.store.pipe(select(allCustomers));
        this.refresh();
    }
    
    addCustomer(customer: Customer) {
        this.store.dispatch(new Create(Customer, customer));
    }
    
    refresh() {
        this.store.dispatch(new LoadAll(Customer));
    }
    
    // ...
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Aside from no longer having to implement your own actions, everything else should be business as usual. Your controllers, when dispatching generic actions from Auto-Entity directly, should look quite familiar. 

### Standardized Properties

Each generic action's constructors require a standard set of parameters, and some actions may require unique parameters to perform the necessary function. Action constructors are as follows:

```typescript
constructor(type: { new (): TModel }, public criteria?: any)
constructor(type: { new (): TModel }, public keys: any, public criteria?: any)
constructor(type: { new (): TModel }, public page: Page, public criteria?: any)
constructor(type: { new (): TModel }, public range: Range, public criteria?: any)
constructor(type: { new (): TModel }, public entity: TModel, public criteria?: any)
constructor(type: { new (): TModel }, public entities: TModel[], public criteria?: any)
```

 Note that the first parameter for every action is the **model type** itself. Also take note that every action accepts optional **custom** **criteria** as the final parameter. This same criteria is later included in the arguments for each entity service method, and is transferred from the action to the entity service method _exactly as provided_ when originally dispatching the action. 

