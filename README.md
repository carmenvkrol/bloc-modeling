##Elevator Modeling Problem##
*Model elevators in a skyscraper*

I'm starting by considering the actions that need to take place, including the inputs and outputs:

1. when the elevator stops on a floor to let off passengers, we need a function to determine whether the elevator can take more passengers.  Along with this, the function should also indicate to the elevator when to open and close its doors.
2. when a user requests the elevator, we need a function that determines which elevator picks up the user


This is the function to deal with #1. I will define the variables later:

```
function capacity (inPeople, outPeople) {
  if((currentPeople+inPeople-outPeople) <= maxCapacity) {
    doorOpen = true;
    currentPeople += inPeople-outPeople;
    setTimeout(function(){
      doorOpen = false;
    }, 30000);   
  } else {
    doorOpen = false;
  } 
  return doorOpen;
};
```


This is the function to deal with #2:

```

function pickUp (floorUser, directionUser) {
    allElevators.every(function(elevator) {
        if (elevator.directionElevator==='up' && directionUser==='up' && (floorUser-elevator.currentFloor)>=0){
            elevator.takeUser = true; 
            return false; // returning false in order to break every function
        } else if (elevator.directionElevator==='down' && directionUser==='down' && (floorUser-elevator.currentFloor)<=0){
            elevator.takeUser = true;
            return false; // returning false in order to break every function
        } else {
           elevator.takeUser = false;       
        }
    });   
}
```

Next, I want to consider the structure of the data.

*Constructor Function for Elevator*

```
var Elevator = {
    
    this.maxCapacity = 10; //10 indicates number of people

    this.capacity = function (inPeople, outPeople) {
      if((this.currentPeople+inPeople-outPeople) <= this.maxCapacity) {
          this.doorOpen = true;
          this.currentPeople += inPeople-outPeople;
          setTimeout(function(){
              this.doorOpen = false;
          }, 30000);   
      } else {
          this.doorOpen = false;
      } 
      return this.doorOpen;
    };

}
```

*Prototype to Initialize Elevator*

```
Elevator.prototype.initialize = function() {
      this.doorsOpen = false;
      this.currentFloor = 0;
      this.currentPeople = 0;
      this.directionElevator = 'up';
      this.takeUser = false;
}
```

In order for the pickUp function to search through all the elevators, there needs to be an array to store these elevators and a function to add a new elevator to this array.

*All the Elevators*

```
var allElevators = []; //array to store all the elevators

function newElevator () { 
    allElevators.push( new Elevator() );
    return allElevators;
}
```

This code is at [this JSFiddle](http://jsfiddle.net/bcapxypy/2/).