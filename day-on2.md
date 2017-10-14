# Stacks&Queues

Twoofthemostcommonlyuseddatastructuresinwebdevelopmentarestacksandqueues.ManyusersoftheInternet,includingwebdevelopers,areunawareofthisamazingfact.Ifyouareoneofthesedevelopers,thenprepareyourselffortwoenlighteningexamples:the'undo'operationofatexteditorusesastacktoorganizedata;theevent-loopofawebbrowser,whichhandlesevents\(clicks,hoovers,etc.\),usesaqueuetoprocessdata.

---

# Stacks

The**last**item added**into**stack will be the**first**one taken**out**of the stack \(**LIFO**\). Stack is basically array an array except that the only thing you can do on it more or less is to push and pop.

Incomputerscience,astackisalineardatastructure.Ifthisstatementholdsmarginalvaluetoyou,asitoriginallydidwithme,considerthisalternative:Astackorganizesdataintosequentialorder.

Thissequentialorderiscommonlydescribedasastackofdishesatacafeteria.Whenaplateisaddedtoastackofdishes,theplateretainstheorderofwhenitwasadded;moreover,whenaplateisadded,itispushedtowardsthebottomofastack.Everytimeweaddanewplate,theplateispushedtowardsthebottomofthestack,butitalsorepresentsthetopofthestackofplates.

Thisprocessofaddingplateswillretainthesequentialorderofwheneachplatewasaddedintothestack.Removingplatesfromastackwillalsoretainthesequentialorderofallplates.Ifaplateisremovedfromthetopofastack,everyotherplateinthestackwillstillretainthecorrectorderinthestack.WhatIamdescribing,possiblywithtoomanywords,ishowplatesareaddedandremovedatmostcafeterias!

Toprovideamoretechnicalexampleofastack,letusrecallthe'undo'operationofatexteditor.Everytimetextisaddedtoatexteditor,thistextispushedintoastack.Thefirstadditiontothetexteditorrepresentsthebottomofthestack;themostrecentchangerepresentsthetopofthestack.Ifauserwantstoundothemostrecentchange,thetopofthestackisremoved.Thisprocesscanberepeateduntiltherearenomoreadditionstothestack,whichisablankfile!

#### OperationsofaStack

Sincewenowhaveaconceptualmodelofastack,letusdefinethetwooperationsofastack:

* `push(data)`adds data.

* `pop()`removes the most recently added data.

#### PropertiesofaStack

For our implementation, we will create a constructor named`Stack`. Each instance of`Stack`will have two properties:`_size`and`_storage`.

\[/\]\(/\)

```
function Stack() {
    this._size = 0;
    this._storage = {};
}
```

`this._storage`enables each instance of`Stack`to have its own container for storing data;

`this._size`reflects the number of times data was pushed to the current version of a`Stack`. If a new instance of`Stack`is created and data is pushed into its storage, then`this._size`will increase to 1. If data is pushed, again, into the stack,

`this._size`will increase to 2. If data is removed from the stack, then`this._size`will decrease to 1.

#### MethodsofaStack

Weneedtodefinemethodsthatcanadd\(push\)andremove\(pop\)datafromastack.Let'sstartwithpushingdata.

**Method1of2:**

`push(data)`\(This method can be shared among all instances of`Stack`, so we'll add it to the prototype of`Stack`.\)

Wehavetworequirementsforthismethod:

1. Every time we add data, we want to increment the size of our stack.
2. Every time we add data, we want to retain the order in which it was added.

```
Stack.prototype.push = function(data) {
    // increases the size of our storage
    var size = this._size++;

    // assigns size as a key of storage
    // assigns data as the value of this key
    this._storage[size] = data;
};
```

**Method2of2:**`pop()`

Wecannowpushdatatoastack;thenextlogicalstepispopping\(removing\)datafromastack.Poppingdatafromastackisnotsimplyremovingdata;itisremovingonlythemostrecentlyaddeddata.

Hereareourgoalsforthismethod:

1. Use a stack's current size to get the most recently added data.
2. Delete the most recently added data.
3. Decrement
   `_this._size`
   by one.
4. Return the most recently deleted data.

```
Stack.prototype.pop = function() {
    var size = this._size,
        deletedData;

    deletedData = this._storage[size];

    delete this._storage[size];
    this.size--;

    return deletedData;
};
```

`pop()`meets each of our four goals. First, we declare two variables:`size`is initialized to the size of a stack;`deletedData`is assigned to the data most recently added to a stack. Second, we delete the key-value pair of our most recently added data. Third, we decrement the size of a stack by 1. Fourth, we return the data that was removed from the stack.

If we test our current implementation of`pop()`, we find that it works for the following use-case. If we`push(data)`data to a stack, the size of the stack increments by one. If we`pop()`data from our stack, the size of our stack decrements by one.

A problem arises, however, when we reverse the order of invocation. Consider the following scenario: we invoke`pop()`and then`push(data)`. The size of our stack changes to -1 and then to 0. But the correct size of our stack is 1!

To handle this use case, we will add an`if`statement to`pop()`.

```
Stack.prototype.pop = function() {
    var size = this._size,
        deletedData;

    if (size) {
        deletedData = this._storage[size];

        delete this._storage[size];
        this._size--;

        return deletedData;
    }
};
```

With the addition of our`if`statement, the body of our code is executed only when there is data in our storage.

#### CompleteImplementationofaStack

Our implementation of`Stack`is complete. Regardless of the order in which we invoke either of our methods, our code works! Here is the final version of our code:

```
function Stack() {
    this._size = 0;
    this._storage = {};
}

Stack.prototype.push = function(data) {
    var size = ++this._size;
    this._storage[size] = data;
};

Stack.prototype.pop = function() {
    var size = this._size,
        deletedData;

    if (size) {
        deletedData = this._storage[size];

        delete this._storage[size];
        this._size--;

        return deletedData;
    }
};
```

---

## FromStacktoQueue

Astackisusefulwhenwewanttoadddatainsequentialorderandremovedata.Basedonitsdefinition,astackcanremoveonlythemostrecentlyaddeddata.Whathappensifwewanttoremovetheoldestdata?Wewanttouseadatastructurenamedqueue.

---

## Queues

Similartoastack,aqueueisalineardatastructure.Unlikeastack,aqueuedeletesonlytheoldestaddeddata.

Tohelpyouconceptualizehowthiswouldwork,let'stakeamomenttouseananalogy.Imagineaqueuebeingverysimilartotheticketingsystemofadeli.Eachcustomertakesaticketandisservedwhentheirnumberiscalled.Thecustomerwhotakesthefirstticketshouldbeservedfirst.

Let'sfurtherimaginethatthistickethasthenumber"one"displayedonit.Thenexttickethasthenumber"two"displayedonit.Thecustomerwhotakesthesecondticketwillbeservedsecond.\(Ifourticketingsystemoperatedlikeastack,thecustomerwhoenteredthestackfirstwouldbethelasttobeserved!\)

Amorepracticalexampleofaqueueistheevent-loopofawebbrowser.Asdifferenteventsarebeingtriggered,suchastheclickofabutton,theyareaddedtoanevent-loop'squeueandhandledintheordertheyenteredthequeue.

### OperationsofaQueue

Sincewenowhaveaconceptualmodelofaqueue,letusdefineitsoperations.Asyouwillnotice,theoperationsofaqueueareverysimilartoastack.Thedifferenceliesinwheredataisremoved.

* `enqueue(data)`
  adds data to a queue.
* `dequeue`
  removes the oldest added data to a queue.

### ImplementationofaQueue

Nowletuswritethecodeforaqueue!

#### PropertiesofaQueue

For our implementation, we will create a constructor named`Queue`. We will then add three properties:`_oldestIndex`,`_newestIndex`, and`_storage`. The need for both`_oldestIndex`and`_newestIndex`will become clearer during the next section.

```
function Queue() {
    this._oldestIndex = 1;
    this._newestIndex = 1;
    this._storage = {};
}
```

#### MethodsofaQueue

We will now create the three methods shared amongst all instances of a queue:`size()`,`enqueue(data)`, and`dequeue(data)`. I will outline the objectives for each method, reveal the code for each method, and then explain the code for each method.

**Method1of3:**`size()`

Wehavetwoobjectivesforthismethod:

1. Return the correct size for a queue.
2. Retain the correct range of keys for a queue.

```
Queue.prototype.size = function() {
    return this._newestIndex - this._oldestIndex;
};
```

Implementing`size()`might appear trivial, but you will quickly find that to be untrue. To understand why, we must quickly revisit how`size`was implemented for a stack.

Usingourconceptualmodelofastack,let'simaginethatwepushfiveplatesontoastack.Thesizeofourstackisfiveandeachplatehasanumberassociatedwithitfromone\(firstaddedplate\)tofive\(lastaddedplate\).Ifweremovethreeplates,thenwehavetwoplates.Wecansimplysubtractthreefromfivetogetthecorrectsize,whichistwo.Here'sthemostimportantpointaboutthesizeofastack:Thecurrentsizerepresentsthecorrectkeyassociatedwiththeplateattopofthestack\(2\)andtheotherplateinthestack\(1\).Inotherwords,therangeofkeysisalwaysfromthecurrentsizeto1.

Now, let's apply this implementation of stack's`size`to our queue. Imagine that five customers take a ticket from our ticketing system. The first customer has a ticket displaying the number 1 and the fifth customer has a ticket displaying the number 5. With a queue, the customer with the first ticket is served first.

Let'snowimaginethatthefirstcustomerisservedandthatthisticketisremovedfromthequeue.Similartoastack,wecangetthecorrectsizeofourqueuebysubtracting1from5.Ourqueuecurrentlyhasfourunservedtickets.Now,thisiswhereaproblemarises:thesizenolongerrepresentsthecorrectticketnumbers.Ifwesimplysubtractedonefromfive,wewouldhaveasizeof4.Wecannotuse4todeterminethecurrentrangeofremainingticketsinthequeue.Dowehaveticketsinthequeuewiththenumbersfrom1to4orfrom2to5?Theanswerisunclear.

This is the benefit of having the following two properties in a queue:`_oldestIndex`and`_newestIndex`. All of this may seem confusing—I'm still occasionally confused. What helps me rationalize everything is the following example I've developed.

Imaginethatourdelihastwoticketingsystems:

1. `_newestIndex`
   represents a ticket from a customer ticketing system.
2. `_oldestIndex`
   represents a ticket from an employee ticketing system.

Here'sthehardestconcepttograspinregardstohavingtwoticketingsystems:Whenthenumbersinbothsystemsareidentical,everycustomerinthequeuehasbeenaddressedandthequeueisempty.Wewillusethefollowingscenariotoreinforcethislogic:

1. A customer takes a ticket. The customer's ticket number, which is retrieved from
   `_newestIndex`
   , is 1. The next ticket available in the customer ticket system is 2.
2. An employee does not take a ticket, and the current ticket in the employee ticket system is 1.
3. We take the current ticket number in the customer system \(2\) and subtract the number in the employee system \(1\) to get the number 1. The number 1 represents the number of tickets still in the queue that have not been removed.
4. The employee takes a ticket from their ticketing system. This ticket represents the customer ticket being served. The ticket that was served is retrieved from
   `_oldestIndex`
   , which displays the number 1.
5. We repeat step 4, and now the difference is zero—there are no more tickets in the queue!

We now have a property \(`_newestIndex`\) that can tell us the largest number \(key\) assigned in the queue and a property \(`_oldestIndex`\) that can tell us the oldest index number \(key\) in the queue.

We have adequately explored`size()`, so let's now move to`enqueue(data)`.

### **Method2of3:**`enqueue(data)`

For`enqueue`, we have two objectives:

1. Use
   `_newestIndex`
   as a key of
   `this._storage`
   and use any data being added as the value of that key.
2. Increment the value of
   `_newestIndex`
   by 1.

Based on these two objectives, we will create the following implementation of`enqueue(data)`:

```
Queue.prototype.enqueue = function(data) {
    this._storage[this._newestIndex] = data;
    this._newestIndex++;
};
```

The body of this method contains two lines of code. On the first line, we use`this._newestIndex`to create a new key for`this._storage`and assign`data`to it.`this._newestIndex`always starts at 1. On the second line of code, we increment`this._newestIndex`by 1, which updates its value to 2.

That's all the code we need for`enqueue(data)`. Let's now implement`dequeue()`.

### **Method3of3:**`dequeue()`

Herearetheobjectivesforthismethod:

1. Remove the oldest data in a queue.
2. Increment
   `_oldestIndex`
   by one.

```
Queue.prototype.dequeue = function() {
    var oldestIndex = this._oldestIndex,
        deletedData = this._storage[oldestIndex];

    delete this._storage[oldestIndex];
    this._oldestIndex++;

    return deletedData;
};
```

In the body of`dequeue()`, we declare two variables. The first variable,`oldestIndex`, is assigned a queue's current value for`this._oldestIndex`. The second variable,`deletedData`, is assigned the value contained in`this._storage[oldestIndex]`.

Next, we delete the oldest index in the queue. After it is deleted, we increment`this._oldestIndex`by 1. Finally, we return the data we just deleted.

Similar to the problem in our first implementation of`pop()`with a stack, our implementation of`dequeue()`does not handle situations where data is removed before any data is added. We need to create a conditional to handle this use case.

```
Queue.prototype.dequeue = function() {
    var oldestIndex = this._oldestIndex,
        newestIndex = this._newestIndex,
        deletedData;

    if (oldestIndex !== newestIndex) {
        deletedData = this._storage[oldestIndex];
        delete this._storage[oldestIndex];
        this._oldestIndex++;

        return deletedData;
    }
};
```

Whenever the values of`oldestIndex`and`newestIndex`are not equal, then we execute the logic we had before.

#### CompleteImplementationofaQueue

Ourimplementationofaqueueiscomplete.Let'sviewtheentirecode.

```
function Queue() {
    this._oldestIndex = 1;
    this._newestIndex = 1;
    this._storage = {};
}

Queue.prototype.size = function() {
    return this._newestIndex - this._oldestIndex;
};

Queue.prototype.enqueue = function(data) {
    this._storage[this._newestIndex] = data;
    this._newestIndex++;
};

Queue.prototype.dequeue = function() {
    var oldestIndex = this._oldestIndex,
        newestIndex = this._newestIndex,
        deletedData;

    if (oldestIndex !== newestIndex) {
        deletedData = this._storage[oldestIndex];
        delete this._storage[oldestIndex];
        this._oldestIndex++;

        return deletedData;
    }
};
```

## Conclusion

Inthisarticle,we'veexploredtwolineardatastructures:stacksandqueues.Astackstoresdatainsequentialorderandremovesthemostrecentlyaddeddata;aqueuestoresdatainsequentialorderbutremovestheoldestaddeddata.

Iftheimplementationofthesedatastructuresseemstrivial,remindyourselfofthepurposeofdatastructures.Theyaren'tdesignedtobeoverlycomplicated;theyaredesignedtohelpusorganizedata.Inthiscontext,ifyoufindyourselfwithdatathatneedstobeorganizedinsequentialorder,considerusingastackorqueue.

