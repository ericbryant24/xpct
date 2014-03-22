xpt
===

Code Contracts in JavaScript

####Ensure correctness and catch those pesky null refs with xpt!

#####Before xpt:

    function printName(first, middle, last) {
      if(!first) {
        throw 'First name must be defined';
      }
      if(!middle) {
        throw 'middle name must be defined';
      }
      if(!last) {
        throw 'last name must be defined';
      }
      return first + middle + last;
    }
    
#####After xpt:

    function printName(first, middle, last) {
      xpt().toBeDef(first, 'First name must be defined')
           .toBeDef(middle, 'Middle name must be defined')
           .toBeDef(last, 'Last name must be defined')
      return first + middle + last;
    }
    
#####Before xpt

    function getCustomerFirstName(order) {
      if(order && order.customer) {
        return order.customer.firstName;
      } else {
        return '';
      }
    }
    
####After xpt

    function getCustomerFirstName(order) {
      return xpt().toBeDef([order, order.customer]).orReturn('')
        .ret(order.customer.firstName)
    }
      
#####Before xpt

    function getSoldOrders() {
      var orders = service.getAllOrders();
      var soldOrders = [];
      
      if(orders) {
        for(var i = 0; i < orders.length(); i++) {
          if(orders[i].sold) {
            soldOrders.push(order[i]);
          }
        }
        return soldOrders;
      } else {
        return []
      }
    }
    
####After xpt

    function getSoldOrders() {
      var orders = service.getAllOrders();
     
      return xpt().toBeArray(orders).orReturn([])
        .go(function() {
          var soldOrders = [];
          for(var i = 0; i < orders.length(); i++) {
            if(orders[i].sold) {
              soldOrders.push(order[i]);
            }
          }
          return soldOrders;
        });
      }
      
