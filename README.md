## Sort Algorithms

If you think about it, a lot of what we ask computers to do involves placing items in the correct order.  For example, if you have a recommendation system like Netflix, these recommendations have should be delivered in the correct order.  But something more basic like displaying apartment listings from highest to lowest also involves sorting.  


### Benefits of Sorted Array

In previous lessons on algorithms, we discussed how much easier other problems get if our collection is sorted.  For example, if we have to see if an array includes the number one, and our array is sorted, we know that after guessing that a number may be at a random index, we know if we need to look higher or lower.  We called this technique **binary search**.  If we guess incorrectly with an unsorted array, we have only ruled out one location where the number cannot be.

```javascript
  let sortedArray = [-1, 1, 3, 5, 6]
  array[2]
  // 3
  // so know need to look lower, only need to look at indices 0 and 1

  let unsortedArray = [5, 6 -1, 1, 3]
  array[2]
  // -1
  // so guess again
```

However there are other operations that become easier with a sorted array.
  * minimum
  * maxiumum
  * median

So, now that we know some of the benefits, how do we do it.

### Selection Sort

```javascript
let unsortedArray = [5, 6 -1, 1, 3]

```

Ok, so given the unsorted array above, if we asked you to place the numbers from lowest to highest could you do it?  What would be the first step?  So go ahead and sort the algorithm, then describe in words what your brain is doing, and finally we'll translate these words into code.

```javascript

  // Think

  // Think

  // Think
  let unsortedArray = [5, 6 -1, 1, 3]

```

Ok, so perhaps what you did was first find the minimum number, remove the minimum from the array, then find the minimum of the remaining numbers, and so on until there are no numbers left in the array.  

```javascript

  let unsortedArray = [5, 6 -1, 1, 3]

  // findthemin given our original array, and remove that element

  // now with the new array, findthemin again, and remove that element

  // keep doing this until our unsortedArray is empty

  // Push these removed elements into an empty array one by one, and that empty array will be our sorted array.
```

Now translate this into code.  

> We still don't know how to find that minimum and remove the element, but we can pretend we had that function, and then write it later.

```javascript
  let sortedArray = []
  let unsortedArray = [5, 6, -1, 1, 3]
  minAndRemove(unsortedArray, sortedArray)
  sortedArray
    // [-1]
  unsortedArray
  // [5, 6, 1, 3]
  minAndRemove(unsortedArray, sortedArray)
  sortedArray
  // [-1, 1]
  unsortedArray
  // [5, 6, 3]
```

And how do we write our minAndRemove function? Well we just go through the numbers one by one, holding onto the smallest number.  We set our min equal to the first element in the array, as we know ultimately it will be replaced by the true minimum.

```javascript
  function minAndRemove(array){
    let min = array[0]
    let minIndex = 0
    for(let i = 0; i < array.length; i++){
      let currentElement = array[i]
      if(array[i] < min){
        min = array[i]
        minIndex = i
      }
    }
    array.splice(minIndex, 1);
    return min;
  }
```

Now check that this piece of code is working.

```javascript
  let unsortedArray = [5, 6, -1, 1, 3]
  minAndRemove(unsortedArray)
  // -1
  unsortedArray
  // [5, 6, 1, 3]
```

Now placing this together, we said that we must call this until there are no other elements left in this array.  Again, write out the words, and then translate it into code.


```javascript
  function selectionSort(array){
    let newMin;
    let sorted = []
    while(array.length != 0){
      newMin = minAndRemove(array)
      sorted.push(newMin)
    }
    return sorted;
  }
```

### Lessons Learned

We just learned how to implement selection sort.  But we also learned that by following the correct process, we don't need to rely on memorization - which generally is unreliable.  Also, we can take this process with us to other problems we haven't seen.  What was our process?

  A. Problem Solving
    1. Give ourselves an example
    2. See if our brain can solve the problem
    3. Think about how our brain is able to solve the problem, slow down and translate this into words

  B. Coding in small steps
    1. After writing these words, translate these words into code
    2. It's ok to call on functions that we need to write later
    3. After getting one piece of the code working, test that the small piece of code works

Yes, this process is a lot to remember.  So it's best not to remember it.  It's better to be disciplined about following this process, whenever you get stuck.  Eventually, it will become a habit.  We'll build more into our process as we go on. Good developers don't lean on their intelligence to solve a problem - that's too hard to improve - they lean on their process.  That, more than knowing the ins and outs of a language - is what makes them good.

### What's the big O

First, let's summarize our process for selection sort: find the minimum and remove elements until there are no more elements left in the original array.  How many times do we have to call our minAndRemove function, well one for each element in the array (as we remove one element each time, and keep going until the array is empty).  Ok, so n times, we incur a cost of minAndRemove.

Now that we have broken this problem down, let's figure out the cost of minAndRemove.  In the minAndRemove function, we go through each element, and then remove it.  

```javascript
function minAndRemove(array){
  let min = array[0]
  let minIndex = 0
  for(let i = 0; i < array.length; i++){
    let currentElement = array[i]
    if(array[i] < min){
      min = array[i]
      minIndex = i
    }
  }
  array.splice(minIndex, 1);
  return min;
}

```
So the cost of minAndRemove is n.  

> Note: If you remember, we said that removing elements from an array costs us n.  But note that we only remove the element one time, at the very end.  So our big O would be n + n = 2n.  And because we can simplify big o to not include what we multiply by, we say that the big o of minAndRemove is n.  

And we said previously that we have to call minAndRemove n times.

```javascript
  function selectionSort(array){
    let newMin;
    let sorted = []
    while(array.length != 0){
      newMin = minAndRemove(array)
      sorted.push(newMin)
    }
    return sorted;
  }
```

So the cost of selection sort is n squared.  Which, we could have derived at by just counting the total number of loops.

  > Note: A subtle point only for the bothered.  Some may ask whether this cost should be less than n^2.  After all, it seems that each time we call minAndRemove on a smaller array, so the cost of minAndRemove shouldn't be n each time, but rather be one less than it was previously.  Let's continue with this logic - it's technically correct.  The first time we call minAndRemove the cost is n, the second time we call it the cost is n - 1, all the way down until the cost is one.  

  > On average though the cost of minAndRemove is (1/2)*n.  This is because the first time the cost is n, and the last time it is zero.  Now remember that with big O, we don't consider the multipliers, so the cost of minAndRemove is still n, and therefore the cost of selection sort is n^2.

### Summary

How to sort an array is a common problem that computers must solve.  We saw that we can sort an array through selection sort, simply by following the process that our brains would generally do.  That is continue to find the minimum of our starting array and remove elements until there are no elements left.  We find the minimum of an unsorted array simply by traversing the elements one by one.

In finding the big O, because the cost of finding the minimum of an unsorted array is n, and we must find the minimum n times, our cost of selection sort is n*n or n^2.  
