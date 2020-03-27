## Week 11 - Day 1
### Case Studies of Locked Data Structures
#### Study 1: A Counter
```
int value; void init(){
    value = 0; 
}
void increment(){ 
    value++;
}
void decrement(){
    value—; 
}
```

There are race conditions in everything but init();  
The Simple solution is to lock each operation

```
int value; 
std::mutex m; 
void init(){
    value = 0; 
}
void increment(){ 
    m.lock();
    value++;
    m.unlock(); 
}
void decrement(){ 
    m.lock();
    value--;
    m.unlock(); 
}
```
This does make it a “safe” counter.  
However it is very slow, and gets slower the more threads get added. Thread creation, locking and unlocking has a cost

#### Study 2: A Linked List - In C
```
typedef struct __node_t{ 
    int key;
    struct __node_t *next;
} node_t;

typedef struct __list_it{ 
    node_t *head;
} list_t;

void init(list_t *L){ 
    L->head = NULL;
}

int insert(list_t *L, int key){
    node_t new_node = malloc(sizeof(node_t)); 
    If (new_node == NULL){
        return -1;
    }
    new_node->key = key;
    new_node->next = L->head;
    L->head = new_node;
    return 0;
}
```
Possible race conditions within this are around creating a new node, and updating the list's pointers

#### Study 3: Concurrent Linked List 
##### V1
```
typedef struct __node_t{ 
    int key;
    struct __node_t *next;
} node_t;

typedef struct __list_it{ 
    node_t *head;
} list_t;

void init(list_t *L){ 
    L->head = NULL;
}

int insert(list_t *L, int key){
    m.lock() // inserting a node needs to be thread safe
    node_t new_node = malloc(sizeof(node_t)); 
    If (new_node == NULL){
        m.unlock() // if the new node fails, unlock the list
        return -1;
    }
    new_node->key = key;
    new_node->next = L->head;
    L->head = new_node;
    m.unlock(); // once node is added to list unlock the list
    return 0;
}
```
This is thread safe but again inefficient  
This is a **coarse-grain** lock  
With some more detailed understanding of the code, we learn malloc is thread safe, thus doesn’t need to be locked around that. Makes it finer-grained. 

##### V2
```
typedef struct __node_t{ 
    int key;
    struct __node_t *next;
} node_t;

typedef struct __list_it{ 
    node_t *head;
} list_t;

void init(list_t *L){ 
    L->head = NULL;
}

int insert(list_t *L, int key){
    node_t new_node = malloc(sizeof(node_t)); 
    If (new_node == NULL){
        return -1;
    }
    new_node->key = key;
    m.lock()
    new_node->next = L->head;
    L->head = new_node;
    m.unlock(); // once node is added to list unlock the list
    return 0;
}
```
This is much more efficient, we only lock around the unsafe section of code. 

#### Study 4: Concurrent Linked List Look Up
```
int lookup(list_t *L, int key){ 
    int return_value = -1;
    m.lock();
    node_t curr = L->head; 
    while (curr){
        if(curr->key = key){
            m.unlock();
            return_value =0;
            break; 
        }
        curr = curr -> next; 
    }
    m.unlock();
    return return_value; 
}
```

We’re locking the whole list before we go through it. That is massively excessive. We only need to lock on the node to see if it has the key.  
This is definitely a coarse-grained locking strategy.

#### Hand-over-Hand Locking
Much more fine-grained  
Lock each node as we traverse the list.  
When done with a node, unlock it and lock the next one in turn.  
If locks can be implemented with low overhead, this is great.  
In practice, this is too fine-grained and will be expensive.  
Lock once every few times? Possible - hybrid locking technique. An open question for best practice

#### Study 5: Michael and Scott Two-Lock Concurrent Queue
Represents queue as a singly-linked list with head and tail pointers.  
Use a single lock for enqueue, and a separate lock for dequeue.  
Head always points to a dummy node. Dummy node is the first node in the list.  
Nodes are inserted after the last node of the list.  
Nodes are deleted from the beginning of the list.  
Initially, both head and tail point to the dummy variable. Without this it can create a deadlock where everything is locked up  
If we didn’t have the dummy variable, they would point at each other.  
This would cause problems if enqueue and dequeue tried to happen simultaneously.  
Two locks + two threads = potential for “deadlock” or “livelock”.  
As we enqueue stuff, we update tail pointer.  
As we dequeue stuff, we update the head pointer.  

```
typedef struct __node_t{ 
    int key;
    struct __node_t *next;
} node_t;

typedef struct __queue_t{ 
    node_t *head;
    node_t *tail;
    std::mutex head_m, tail_m; 
} queue_t;

void init(queue_t *q){
    node_t *tmp = malloc(sizeof(node_t)); 
    tmp->next = NULL;
    q->head = q->tail = tmp;
}

void enqueue(queue_t *q, int value){ 
    node_t *tmp = malloc(sizeof(node_t)); 
    assert(tmp!=NULL);
    tmp->value = value;
    tmp->next = NULL;
    tail_m.lock();
    q->tail->next = tmp; 
    q->tail = tmp; 
    tail_m.unlock();
}

int dequeue(queue_t *q, int *value){
    head_m.lock();
    node_t *tmp = q->head;
    node_t *new_head = tmp->next; 
    if (new_head==NULL){
        head_m.unlock();
        return -1; 
    }
    *value = new_head->value;
    q->head = new_head; 
    head_m.unlock();
    return 0;
}
```
Enqueue is a nice example of fine-grained locking.  
The dummy variable is a great example of how serial data structures must be modified for concurrency, beyond just adding locks.  
Michael and Scott show that two-lock queue scales very well and avoids deadlock/livelock.  
If you use scoped locks, the lock will automatically unlock when it goes out of scope.  
In C++, use curly braces to define scopes.  
Allows for unlock even after return!

#### Study 6: Concurrent Hash Table
A hash table without resizing  
We’ll use separate chaining instead of probing. This means an array of linked lists.  
Each “bucket” in the table is now a linked list.  
Collisions are resolved by hashing to the same bucket, then growing the corresponding linked list.  
This can use the same concurrent linked list seen earlier
list_t table[BUCKETS].  
Hash table insert is just a call to the list_t insert function.  
Lookup is a call to the list_t lookup.

### Summary
Coarse-grained locking (locking large chunks of code) is easy, but inefficient.  
Fine-grained locking is trickier to get right without deadlock/livelock. It may need to redesign data structure or code. It may still scale poorly  
Enabling concurrency doesn’t always improve performance!  
Sometimes all we can do is hope it doesn’t degrade it.
