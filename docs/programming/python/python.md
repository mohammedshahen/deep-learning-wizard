
# Python

## Lambda, map, filter, reduce

### Lambda

#### Add Function


```python
add = lambda x, y: x + y
add(2, 3)
```




    5



#### Multiply Function


```python
multiply = lambda x, y: x * y 
multiply(2, 3)
```




    6



### Map

#### Create List


```python
lst = [i for i in range(11)]
print(lst)
```

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]


#### Map Square Function to List


```python
square_element = map(lambda x: x**2, lst)

# This gives you a map object
print(square_element)

# You need to explicitly return a list
print(list(square_element))
```

    <map object at 0x7f9cd4399a58>
    [0, 1, 4, 9, 16, 25, 36, 49, 64, 81, 100]


#### Create Multiple List


```python
lst_1 = [1, 2, 3, 4]
lst_2 = [2, 4, 6, 8]
lst_3 = [3, 6, 9, 12]
```

#### Map Add Function to Multiple Lists


```python
add_elements = map(lambda x, y, z : x + y + z, lst_1, lst_2, lst_3)
print(list(add_elements))
```

    [6, 12, 18, 24]


### Filter

#### Create List


```python
lst = [i for i in range(10)]
print(lst)
```

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]


#### Filter multiples of 3


```python
multiples_of_three = filter(lambda x: x % 3 == 0, lst)
print(list(multiples_of_three))
```

    [0, 3, 6, 9]


### Reduce
The syntax is `reduce(function, sequence)`. The function is applied to the elements in the list in a sequential manner. Meaning if `lst = [1, 2, 3, 4]` and you have a sum function, you would arrive with `((1+2) + 3) + 4`.


```python
from functools import reduce
sum_all = reduce(lambda x, y: x + y, lst)
# Here we've 1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9
print(sum_all)
print(1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9)
```

    45
    45


## Decorators
- This allows us to to modify our original function or even entirely replace it without changing the function's code. 
- It sounds mind-boggling, but a simple case I would like to illustrate here is using decorators for consistent logging (formatted print statements).
- For us to understand decorators, we'll first need to understand:
    - `first class objects`
    - `*args`
    - `*kwargs`

### First Class Objects


```python
def outer():
    def inner():
        print('Inside inner() function.')
        
    # This returns a function.
    return inner

# Here, we are assigning `outer()` function to the object `call_outer`.
call_outer = outer()

# Then we call `call_outer()` 
call_outer()
```

    Inside inner() function.


### *args
-  This is used to indicate that positional arguments should be stored in the variable args
- `*` is for iterables and positional parameters


```python
# Define dummy function
def dummy_func(*args):
    print(args)
    
# * allows us to extract positional variables from an iterable when we are calling a function
dummy_func(*range(10))

# If we do not use *, this would happen
dummy_func(range(10))

# See how we can have varying arguments?
dummy_func(*range(2))
```

    (0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
    (range(0, 10),)
    (0, 1)


### **kwargs
- `**` is for dictionaries & key/value pairs


```python
# New dummy function
def dummy_func_new(**kwargs):
    print(kwargs)
    
# Call function with no arguments
dummy_func_new()

# Call function with 2 arguments
dummy_func_new(a=0, b=1)

# Again, there's no limit to the number of arguments.
dummy_func_new(a=0, b=1, c=2)

# Or we can just pass the whole dictionary object if we want
new_dict = {'a': 0, 'b': 1, 'c': 2, 'd': 3}
dummy_func_new(**new_dict)
```

    {}
    {'a': 0, 'b': 1}
    {'a': 0, 'b': 1, 'c': 2}
    {'a': 0, 'b': 1, 'c': 2, 'd': 3}


### Decorators as Logger and Debugging


```python
# Create a nested function that will be our decorator
def function_inspector(func):
    def inner(*args, **kwargs):
        result = func(*args, **kwargs)
        print(f'Function args: {args}')
        print(f'Function kwargs: {kwargs}')
        print(f'Function return result: {result}')
        return result
    return inner

# Decorate our multiply function with our logger for easy logging
# Of arguments pass to the function and results returned
@function_inspector
def multiply_func(num_one, num_two):
    return num_one * num_two

multiply_result = multiply_func(num_one=1, num_two=2)
```

    Function args: ()
    Function kwargs: {'num_one': 1, 'num_two': 2}
    Function return result: 2


## Dates

### Get Current Date


```python
import datetime
now = datetime.datetime.now()
print(now)
```

    2019-06-06 11:26:47.843927


### Get Clean String Current Date


```python
# YYYY-MM-DD
now.date().strftime('20%y-%m-%d')
```




    '2019-06-06'



### Count Business Days


```python
# Number of business days in a month from Jan 2019 to Feb 2019
import numpy as np
days = np.busday_count('2019-01', '2019-02')
print(days)
```

    23


## Progress Bars

### TQDM
Simple progress bar via `pip install tqdm`


```python
from tqdm import tqdm
import time
for i in tqdm(range(100)):
    time.sleep(0.1)
    pass
```

    100%|██████████| 100/100 [00:10<00:00,  9.86it/s]


## Check Paths

### Check Path Exists
- Check if directory exists


```python
import os
directory='new_dir'
print(os.path.exists(directory))

# Magic function to list all folders
!ls -d */
```

    False
    ls: cannot access '*/': No such file or directory


### Check Path Exists Otherwise Create Folder
- Check if directory exists, otherwise make folder


```python
if not os.path.exists(directory):
    os.makedirs(directory)
    
# Magic function to list all folders
!ls -d */

# Remove directory
!rmdir new_dir
```

    new_dir/


## Asynchronous

### Concurrency, Parallelism, Asynchronous
- Concurrency (single CPU core): multiple threads on a single core running in **sequence**, only 1 thread is making progress at any point
    - Think of 1 human, packing a box then wrapping the box
- Parallelism (mutliple GPU cores): multiple threads on multiple cores running in **parallel**, multiple threads can be making progress
    - Think of 2 humans, one packing a box, another wrapping the box
- Asynchronous: concurrency but with a more dynamic system that moves amongst threads more efficiently rather than waiting for a task to finish then moving to the next task
    - Python's `asyncio` allows us to code asynchronously
    - Benefits:
        - Scales better if you need to wait on a lot of processes
            - Less memory (easier in this sense) to wait on thousands of co-routines than running on thousands of threads
        - Good for IO bound uses like reading/saving from databases while subsequently running other computation
        - Easier management than multi-thread processing like in parallel programming
            - In the sense that everything operates sequentially in the same memory space

### Asynchronous Key Components
- The three main parts are (1) coroutines and subroutines, (2) event loops, and (3) future.
    - Co-routine and subroutines
        - Subroutine: the usual function
        - Coroutine: this allows us to maintain states with memory of where things stopped so we can swap amongst subroutines
            - `async` declares a function as a coroutine
            - `await` to call a coroutine
    - Event loops
    - Future

### Synchronous 2 Function Calls


```python
def add_numbers(num_1, num_2):
    print('Adding')
    time.sleep(1)
    return num_1 + num_2

def display_sum(num_1, num_2):
    total_sum = add_numbers(num_1, num_2)
    print(f'Total sum {total_sum}')
    
def main():
    display_sum(2, 2)
    display_sum(2, 2)

start = timeit.default_timer()

main()

end = timeit.default_timer()
total_time = end - start

print(f'Total time {total_time:.2f}s')
```

    Adding
    Total sum 4
    Adding
    Total sum 4
    Total time 2.00s


### Parallel 2 Function Calls


```python
from multiprocessing import Pool
from functools import partial

start = timeit.default_timer()

pool = Pool()
result = pool.map(partial(display_sum, num_2=2), [2, 2]) 

end = timeit.default_timer()
total_time = end - start

print(f'Total time {total_time:.2f}s')
```

    Adding
    Adding
    Total sum 4
    Total sum 4
    Total time 1.12s


### Asynchronous 2 Function Calls
For this use case, it'll take half the time compared to a synchronous application and slightly faster than parallel application (although not always true for parallel except in this case)


```python
import asyncio
import timeit
import time
    
async def add_numbers(num_1, num_2):
    print('Adding')
    await asyncio.sleep(1)
    return num_1 + num_2 

async def display_sum(num_1, num_2):
    total_sum = await add_numbers(num_1, num_2)
    print(f'Total sum {total_sum}')
    
async def main():
    # .gather allows us to group subroutines
    await asyncio.gather(display_sum(2, 2), 
                         display_sum(2, 2))
    
start = timeit.default_timer()

# For .ipynb, event loop already done
await main()

# For .py
# asyncio.run(main())

end = timeit.default_timer()
total_time = end - start

print(f'Total time {total_time:.4f}s')
```

    Adding
    Adding
    Total sum 4
    Total sum 4
    Total time 1.0008s
