xpct
===

Code Contracts in JavaScript

####Ensure correctness and catch those pesky null refs with xpct!

#####Before xpct:

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
    
#####After xpct:

    function printName(first, middle, last) {
      xpct().toBeDef(first, 'First name must be defined')
           .toBeDef(middle, 'Middle name must be defined')
           .toBeDef(last, 'Last name must be defined')
      return first + middle + last;
    }
    
#####Before xpct

    function getCustomerFirstName(order) {
      if(order && order.customer) {
        return order.customer.firstName;
      } else {
        return '';
      }
    }
    
#####After xpct

    function getCustomerFirstName(order) {
      return xpct().toBeDef([order, order.customer]).orReturn('')
        .ret(order.customer.firstName)
    }
      
#####Before xpct

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
    
#####After xpct

    function getSoldOrders() {
      var orders = service.getAllOrders();
     
      return xpct().toBeArray(orders).orReturn([])
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
      
