## Week 9 - Day 2
### Optimize Compilers
Language matters. Each work differently so one change in a language might make things worse in a different language

Efficient mapping of the program to machine

* Register allocation
* instruction selection and ordering scheduling
* Dead code elimination
* Eliminating minor inefficiencies

Don’t (usually) improve asymptotic efficiency (constant factor does but if it is O(N^2) it will stay that way

* Programmer must use proper algorithm
* Non-algorithmic difference can still be a 10x difference
* Compiler helps programer and vice versa 

Optimization must not change the program behavior ... as long as the behavior is defined  
Behavior obvious to the programmer can be obfuscated by coding style  
e.g., actual range narrower than datatype  
Most analysis is performed only within a procedure ... or within a file  
Analysis is based only on static information  
i.e., optimizer doesn’t know program input

Performed by you or by compiler:

* Code motion - move it to a place it is more effective (out of a loop)
* Strength reduction (some operations can be done with cheaper operations)
* Sharing common results (a.k.a. common subexpression elimination)

### Code Motion

```
void set_row(double *a, double *b, long i, long n) { long j;
  for (j = 0; j < n; j++)
    a[n*i+j] = b[j];
}
```

```a[n*i+j] - n*I``` doesn’t need to be called every loop, it is expensive. Save it as a local variable

```
void set_row(double *a, double *b, long i, long n) { long j;
int ni = n*i;
   for (j = 0; j < n; j++)
     a[ni+j] = b[j];
}
```
Compiler will actually move n*I out like this

Compiler will turn above into

```
long ni = n*i;
double *rowp = a+ni;
for (j = 0; j < n; j++)
    *rowp++ = b[j];
```

### Strength Reduction
Bit shifting is cheaper than adding or multiplying 

```
int sixteen(int v) {
  return 16*v;
}

// Becomes
x<<4
```

### Common Subexpressions
```
/* Sum values near i,j */ 
prev2 = val[i*n + j - 2]; 
prev = val[i*n + j - 1]; 
next = val[i*n + j + 1]; 
next2 = val[i*n + j + 2]; 
sum = (prev2 + prev + next + next2);

// Becomes

long inj = i*n + j; 
prev2 = val[inj - 2]; 
prev = val[inj - 1]; 
next = val[inj + 1]; 
next2 = val[inj + 2]; 
sum = (prev2 + prev + next + next2);
```

### Optimization Blocker 1: Procedure Calls
```
int g(int v);
int f(int *a, int len) {
  int i, accum = 0;
  for (i = 0; i < len; i++) {
    accum += a[i];
    accum += g(a[i]);
    accum += a[i];
}
  return accum;
}
```

We/compiler doesn’t know what g(int v) does  
It cannot optimize the a[I] fully. g(int v) might modify the a[I] array. If it does then that changes the execution. Compiler can’t reason past it

### Optimization Blocker 2: Aliasing
```
/* Sum rows of `a` and store in vector `b` */ void sum_rows1(int *a, int *b, long n) {
  long i, j;
  for (i = 0; i < n; i++) {
    b[i] = 0;
    for (j = 0; j < n; j++)
      b[i] += a[i*n + j];
  }
}
```

Compiler cannot pull out the writing to b[I] as b[I] might be the same as a[].  
This is aliasing. To avoid this fix the loop

```
/* Sum rows of `a` and store in vector `b` */ void sum_rows2(int *a, int *b, long n) {
  long i, j;
  for (i = 0; i < n; i++) {
    int s = 0;
    for (j = 0; j < n; j++)
      s += a[i*n + j];
    b[i] = s;
} 
}
```

This can also be avoided with **strict aliasing**

```
int sum_and_set(int *a, long *c, int len) { int i, accum = 0;
    for (i = 0; i < len; i++) {
      accum += a[i];
      c[i] = i;
      accum += a[i];
}
  return accum;
}
```

c[] =/= a[] based on c[] being long and a[] being int  
While it is possible for c = (long *) a, it is a really poor idea and programmers have agreed to keep this as a “rule” to follow

### Optimization Example: Vectors
```
typedef .... data_t;
typedef struct { 
    size_t len; 
    data_t *data;
} vec;

vec *new_vec(size_t len) {
    vec *result = (vec *) malloc(sizeof(vec)); result->len = len;
    result->data = calloc(len, sizeof(data_t)); return result;
}

/* retrieve vector element and store at val */
int get_vec_element(vec *v, size_t idx, data_t *val) {
    if (idx >= v->len) return 0;
        *val = v->data[idx];
    return 1; 
}
```

Thing we care about - The slow function found via profiling

```
void combine1(vec_ptr v, data_t *dest) { long int i;
    *dest = IDENT;
    for (i = 0; i < vec_length(v); i++) {
        data_t val; get_vec_element(v, i, &val); 
        *dest = *dest OP val;
    }
}
```

Timing combine1 with ints, doubles both addition and multiplication, and at different optimization levels

|                     |  INT  |  INT  | DOUBLE | DOUBLE |
|:-------------------:|:-----:|:-----:|:------:|:------:|
|                     |   +   |   *   |    +   |    *   |
|     Without -01     | 22.68 | 20.02 |  19.98 |  20.18 |
|       With -01      | 10.12 | 10.12 |  10.17 |  11.14 |

vec_length(v) has to be calculated in each loop. It can be pulled out.
  
|                     |  INT  |  INT  | DOUBLE | DOUBLE |
|:-------------------:|:-----:|:-----:|:------:|:------:|
|                     |   +   |   *   |    +   |    *   |
|       With -01      | 10.12 | 10.12 |  10.17 |  11.14 |
|   Lift vec_length   |  7.02 |  9.03 |  9.02  |  11.03 |

Because we know where the vector starts, we can pull out that starting location. With length known we stay inbounds. We can remove that function call from being in every loop

|                     |  INT  |  INT  | DOUBLE | DOUBLE |
|:-------------------:|:-----:|:-----:|:------:|:------:|
|                     |   +   |   *   |    +   |    *   |
|   Lift vec_length   |  7.02 |  9.03 |  9.02  |  11.03 |
|     Direct data     |  7.17 |  9.02 |  9.02  |  11.03 |


No apparent change, this is one where the good change effect shows as other changes are made. 
We are changing dest every time, we can pull it out as a local variable
|                     |  INT  |  INT  | DOUBLE | DOUBLE |
|:-------------------:|:-----:|:-----:|:------:|:------:|
|                     |   +   |   *   |    +   |    *   |
|     Direct data     |  7.17 |  9.02 |  9.02  |  11.03 |
| Accumulate to local |  1.27 |  3.01 |  9.02  |  5.01  |

**Final CombineX Code**

```
void combineX(vec_ptr v, data_t *dest) { long int i;
    int length = vec_length(v);
    data_t acc = IDENT;
        for (i = 0; i < length; i++) { 
            data_t val; 
            get_vec_element(v, i, &val); 
            acc = acc OPER val;
        }
    *dest = acc; 
}
```

Speeds ups seen:

|                     |  INT  |  INT  | DOUBLE | DOUBLE |
|:-------------------:|:-----:|:-----:|:------:|:------:|
|                     |   +   |   *   |    +   |    *   |
|     Without -01     | 22.68 | 20.02 |  19.98 |  20.18 |
|       With -01      | 10.12 | 10.12 |  10.17 |  11.14 |
|   Lift vec_length   |  7.02 |  9.03 |  9.02  |  11.03 |
|     Direct data     |  7.17 |  9.02 |  9.02  |  11.03 |
| Accumulate to local |  1.27 |  3.01 |  9.02  |  5.01  |


There are even more optimizations that can be done like multi threading for adding all odd locations and all even locations then totaling in the end