### Reasonable solutions

An iterative solution is fine:

```python
def binary_search(list_of_numbers, number):
    low_index = 0
    high_index = len(list_of_numbers)
    while low_index < high_index:
        mid_index = (low_index + high_index) // 2
        mid_value = list_of_numbers[mid_index]
        if number < mid_value:
            high_index = mid_index
        elif number == mid_value:
            return mid_index
        else:
            low_index = mid_index + 1
    raise ValueError('number not found')
```

And so is a recursive solution:

```python
def binary_search(list_of_numbers, number):
    return binary_search_helper(list_of_numbers, number, 0, len(list_of_numbers))

def binary_search_helper(list_of_numbers, number, low_index, high_index):
    if low_index >= high_index:
        raise ValueError('number not found')
    else:
        mid_index = (low_index + high_index) // 2
        mid_value = list_of_numbers[mid_index]
        if number < mid_value:
            return binary_search_helper(list_of_numbers, number, low_index, mid_index)
        elif number == mid_value:
            return mid_index
        else:
            return binary_search_helper(list_of_numbers, number, mid_index + 1, high_index)
```

### Unreasonable Solutions

The whole point of binary search is to efficiently find a value in the list, in
O(log(n)) time.  Any solution that searches the entire list, or copies even
half the list is missing the point.  This recursive solution gets the logic
right, but copies the remaining list on each recursive call, so it runs in O(n) time:

```python
def binary_search(list_of_numbers, number):
    if len(list_of_numbers) == 0:
        raise ValueError('number not found')
    else:
        mid_index = len(list_of_numbers) // 2
        mid_value = list_of_numbers[mid_index]
        if number < mid_value:
            return binary_search(list_of_numbers[:mid_index], number)
        elif number == mid_value:
            return mid_index
        else:
            return (mid_index + 1) + binary_search(list_of_numbers[mid_index+1:], number)
```

### Common suggestions
- When the code gets the wrong answer, find a small test case that fails and suggest thinking through the logic to see where it goes wrong.

### Talking points
- Using a single variable to track where the midpoint is on each iteration is not enough, because you also need to track how long the current sub-list is.
- You can represent the high end of the range as the last index where the value could be (inclusive), or one past that (exclusive).  Both work.  The exclusive approach seems more natural in Python, because that's the way slicing works.




