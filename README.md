<div align="center">

## What is a linked list?


</div>

### Description

An introduction to linked lists for beginners. I know there are already some. I guess it won't hurt to have another.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[SouperMan](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/souperman.md)
**Level**          |Beginner
**User Rating**    |4.8 (43 globes from 9 users)
**Compatibility**  |C\+\+ \(general\)
**Category**       |[Data Structures](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/data-structures__3-8.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/souperman-what-is-a-linked-list__3-953/archive/master.zip)





### Source Code

```
/*
 Linked Lists
 What is a linked list? Before we answer that question, let me ask a
 different question. Suppose you had a program that kept track of the
 names of employees. How would you store these names? One way would
 be to use an array. You can use something like NameStruct Employees[50],
 where NameStruct was a structure with a first and last name. But what
 happens when you have more than 50 employees? What if you have 100?
 Or what about 1000? What if you won't ever really know?
 You can prepare for the worst case, and create a huge array of say 10,000
 arrays. But what really is the worst case? And more importantly, when
 there are only a few employees, you would be wasting a lot of precious
 memory allocating a bunch of empty NameStruct variables.
 A nice solution to this problem, is a flexible linked list. A linked
 list is a data structure, which can grow and shrink as you want it to.
 You can add items to it, you can access the items on it, and you can
 delete items from it. And the whole time, you only allocate an amount
 of memory proportional to the number of items you have.
 So how does a linked list accomplish this task?
 A linked list takes advantage of the fact that you can store memory
 addresses to other variables. We have structures (sometimes called nodes)
 which contain the information you want. Along with that information,
 is the address to another structure (node).
 Let A, B, and C be NameStruct (nodes). Let X --> Y represent
 the fact that X contains the memory address of Y (the so-called "link").
 A --> B --> C --> Null
 is a linked list with three nodes, terminated with a Null node.
 How do we access the elements? Say we know the address of A, but we
 are looking for the last name associated with someone who has the
 first name "George" (say, C.firstName). We can check what address
 A is storing, and find out it's B. And B.firstName isn't "George".
 So, we move on, and check what address B is storing, and find out
 it's C. And C.firstName is "George" so we have a winner.
 In this manner, we can access any element in the list, just by knowing
 the first node's address. But this isn't the power of the linked list.
 The best part about the linked list is that you can add and remove
 elements, while only using the amount of memory you need. It requires
 some dynamic allocation (which C++ provides with new/delete). However,
 if it's implemented well, most of the details can be hidden nicely.
 Using a linked list should be easy as pie.
 On the other hand, implementing a linked list can be tedious,
 depending on how many features you would like it to have. For example,
 when you add elements, should it be in sorted order?
 Since this is just a simple description of a linked list, there are
 no fancy features.
 So let's go through the code together.
*/
#include <iostream.h>
/* Here is our Node (our structure) that contains the information
  we want along with a memory address to the next Node on the list.
  When the list is empty, our there is no next. In fact, there is
  no Node at all, as you will see in a moment.
*/
class Node
{
public:
 int x;         /* The information we really want to store. */
 Node *next;      /* The address to the next Node in the list. */
 Node() { next = NULL; }       /* Start of with no next Node. */
 /* Here, we want to get rid of the next Node before getting rid of
   this one. Why? It's a tricky way to get rid of all the nodes, by
   simply getting rid of the first once. Think "chain-reaction".
 */
	~Node() { if (next != NULL) delete next; }
};
/* Here is our List, which manages all the Nodes. Using the List
  interface, we can add nodes, remove nodes, or print out the nodes
  that are in the list.
*/
class List
{
 /* The "head" is a variable that points to the first node in our
   list. Keep in mind, "head" isn't a Node itself. It's just the
   memory address of the first actual node. In the beginning, when
   there are no items in our list, "head" doesn't have an address
   and that means head = NULL.
 */
 Node *head;
 /* The "tail" is similar to the "head" except it has the memory
   address of the last actual node. It's not a necessary variable,
   but it's handy for adding elements because we don't have to
   go through the entire chain to find the last element in our
   list.
 */
 Node *tail;
 /* The size of the list will be the number of elements in it. It's
   not really necessary, just something that might come in handy.
 */
 int size;
public:
 List() { size = 0; head = NULL; tail = NULL; }
 /* Deleting the head removes ALL the links in the linked list. Why?
   Think "chain-reaction". The head doesn't disappear until head's
   "next" variable disappears, which doesn't disappear until it's own
   "next" variable disappears, and so on...
 */
 ~List() { delete head; }
 /* Some typical linked list functions */
 void add(int x);
 void del(int x);
 void print();
};
/* How do we add an element to a linked list? We create a new Node using
  new Node (hmm, C++ syntax makes sense I guess). We give it the data
  we want (in this case, just a number). Then, we make the tail's "next"
  variable (which is the last element on the list) be the temp Node.
  We have to take care of the special case when this is the first node
  on the list, otherwise "head" won't be have any "next" address (that is,
  "head" won't be pointing to anything).
*/
void List::add(int x)
{
 Node *temp;
 temp = new Node;
 temp->x = x;
 if (size == 0) {
  head = temp;
  tail = temp;
 } else {
  tail->next = temp;
  tail = temp;
 }
 /* Our list is now one size bigger. */
 size++;
 cout << "List::add(" << temp->x << ")." << endl;
}
/* How do we delete an element from a linked list? Well, for this
  procedure, think "heart bypass surgery". We have a chain of Nodes
  like this:
  A --> B --> C --> D --> E --> Null
  and to delete C, we would simply have B point to D. So we have
  A --> B   C --> D --> E --> Null
     |      |
     \-----------/
  and C is no longer part of the chain. Still, we should get rid of
  the memory allocated for C, and we do that using the delete
  keyword in C++.
  Note: Before we can delete C, we need to let C's "next" variable
  be NULL. Why? Remember, C won't disappear until C's "next" also
  disappears, and so on, and we'll end up getting rid of D and E as
  well. That's not what we want.
  A --> B --------> D --> E --> Null
*/
void List::del(int x) {
 Node *temp = head, *prev = head;
 for (int i=0; i<size; i++)
 {
  if (temp->x == x) {
   prev->next = temp->next;
   if (head == temp)
    head = temp->next;
   temp->next = NULL;
   delete temp;
   i = size;
  }
  else {
   prev = temp;
   temp = temp->next;
  }
 }
 size--;
 cout << "List::del(" << x << ")." << endl;
}
/* How do we print out all of the elements in the list?
  One way to do it, is with a single loop. Use a temporary
  variable, which is reassigned to have the value which it's own
  "next" variable has, during each loop.
  Run the program to see what the output looks like.
*/
void List::print()
{
 Node *temp;
 temp = head;
 cout << "List::print() output: ";
 do {
  cout << "[" << temp->x << "] --> ";
  temp = temp->next;
 } while (temp != NULL);
 cout << "NULL" << endl;
}
/* And here is the great main function. We just do a few simple
  operations on the list, to try out the creation. It's nothing
  really special. Then again, I've always thought that the
  most precious things in life are the simple things in life.
*/
int main(void)
{
 List list;
 list.add(1);
 list.add(2);
 list.add(3);
 list.print();
 list.del(3);
 list.print();
 cout << "Done, press enter." << endl;
 char c;
 cin >> c;
 return 0;
}
```

