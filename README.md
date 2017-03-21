## Merge Sort Algorithm

### Background

We embark on learning about the merge sort algorithm with the hope that we can improve upon selection sort.  When we say improve we mean with the hope that we can find a sorting algorithm that has a big o less than n^2.  

### The Merge Operation

Learning about merge sort begins with a leap of faith.  

https://0lem.files.wordpress.com/2012/03/indiana-leap-of-faith.jpg

The faith is the following.  We will start with an unsorted array.  And ultimately we can turn that unsorted array into two *sorted* subarrays.

```javascript
  let unsortedArray =  [7, 1, 2, 8, 6, 10, 4, 3, 9, 5]


  // sorted subarrays
  let firstHalf =  [1, 2, 6, 7, 8]
  let secondHalf =  [3, 4, 5, 9, 10]
```

Take a look at these two arrays, they are not filled with sequential numbers, but each array is independently sorted.  Later on we will figure out how to get there.  But first, assuming that we have those two sorted subarrays, how would we turn that into an algorithm that produces a sorted array.  We will call this algorithm our merge function.

```javascript
let firstHalf =  [1, 2, 6, 7, 8]
let secondHalf =  [3, 4, 5, 9, 10]

merge(firstHalf, secondHalf)
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

```

Ok, so given these two subarrays, how would you get to the final sorted array.  Hmmmmmmmmmmm...

Ok, well what we could go from the following insight.  The minimum of each subarray has to be at the zero index.  So the minimum of firstHalf is firstHalf[0] and the minimum of secondHalf is secondHalf[0].  Now what is the minimum of the two halves, well its whichever of these two minimums is smaller.  So, given two sorted subarrays, let's find the minimum.  

```javascript
let firstHalf =  [1, 2, 6, 7, 8]
let secondHalf =  [3, 4, 5, 9, 10]

function findMin(firstHalf, secondHalf){
  let minfirstHalf = firstHalf[0]
  let minsecondHalf = secondHalf[0]

  if(minfirstHalf < minsecondHalf){
    return minfirstHalf
  } else {
    return minsecondHalf
  }
}

findMin(firstHalf, secondHalf)
// 1
```

And thinking about how this turns into a sorted array, upon finding the minimum, we can remove the element and push it into another array.  So we are back to writing a findMinAndRemove function, except this time it is for two sorted subarrays.

```javascript

let firstHalf =  [1, 2, 6, 7, 8]
let secondHalf =  [3, 4, 5, 9, 10]

function findMinAndRemove(firstHalf, secondHalf){
  let minfirstHalf = firstHalf[0]
  let minsecondHalf = secondHalf[0]

  if(minfirstHalf < minsecondHalf){
    return firstHalf.shift()
  } else {
    return secondHalf.shift()
  }
}

findMinAndRemove(firstHalf, secondHalf)
// 1

firstHalf
// [2, 6, 7, 8]
```

Now, just like with selection sort, to arrive at our sorted array, we just have to keep calling our findMinAndRemove function until there are no elements left - ie. both subarrays are empty.  We said that we call this process of going from sorted subarrays to a sorted array our merge function.

```javascript
  let firstHalf =  [1, 2, 6, 7, 8]
  let secondHalf =  [3, 4, 5, 9, 10]

  function merge(firstHalf, secondHalf){
    let sorted = []
    let currentMin;
    while(firstHalf.length != 0 && secondHalf.length != 0){
      let currentMin = findMinAndRemove(firstHalf, secondHalf)
      sorted.push(currentMin)
    }
    return sorted.concat(firstHalf).concat(secondHalf)
  }
```

So let's quickly look at the function above.  Our merge function for given two subarrays continues to call findMinAndRemove until one of our arrays is empty.  Then once one of the arrays is empty, we know that other array only has the remaining sorted elements.  So we can simply concatenate the remaining elements onto the end of our array.  So in the example above, when the function hits the return statement, sorted array, firstHalf and secondHalf will look like the following:

```javascript
  sorted
    // [1, 2, 3, 4, 5, 6, 7, 8]
  firstHalf
    // []
  secondHalf
    // [9, 10]
  sorted.concat(firstHalf).concat(secondHalf)
  // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

And what is the big o of merge(firstHalf, secondHalf)

```javascript
function merge(firstHalf, secondHalf){
  let sorted = []
  let currentMin;
  while(firstHalf.length != 0 && secondHalf.length != 0){
    let currentMin = findMinAndRemove(firstHalf, secondHalf)
    sorted.push(currentMin)
  }
  return sorted.concat(firstHalf).concat(secondHalf)
}

```

Well the worse case scenario is that we go through the cost of every element in each array.  Or, to say it a different way, the worse case scenario is that we pass through our while loop once for each element in each array.  We cannot go through the loop more than that, because each time through our while loop we remove an element.  So the cost of merging two subarrays is firstHalf.length + secondHalf.length or simply the original array's length, or n.

Ok, so now that we have written out the merge operation, now let's go back to entire mergeSort function.

```javascript
  function mergeSort(array){
    if(array.length < 2){
      return array
    } else {
      let midpoint = array.length/2
      let firstHalf = array.slice(0, midpoint)
      let secondHalf = array.slice(midpoint, array.length)
      merge(mergeSort(firstHalf), mergeSort(secondHalf))
    }
  }
```

Believe it or not, that is our entire mergeSort function.  

If the array is just one element, mergeSort returns that array.  (An array of length one is automatically sorted)

So now let's try to understand the second half of the function.  

```javascript
  let array =  [1, 2, 6, 7, 8, 3, 4, 5, 9, 10]

  mergeSort(array)
```

So the first thing this will do, is split the array into two halves.

```javascript

function mergeSort(array){
  if(array.length < 2){
    return array
  } else {
    let midpoint = array.length/2
    let firstHalf = array.slice(0, midpoint)
    let secondHalf = array.slice(midpoint, array.length)
    merge(mergeSort(firstHalf), mergeSort(secondHalf))
  }

  // [1, 2, 6, 7, 8]
   // [3, 4, 5, 9, 10]

}
```
Then at the last line of the function, it passes the firstHalf of the array to the merge sort function, and the second half of the array to the merge sort function.  So, this means that until we get to the merge part, are we are doing is splitting down into smaller and smaller components.

```javascript
  let array =  [2, 1, 7, 6, 8, 3, 4, 5]


  mergeSort(array)
  // [2, 1, 7, 6]      [8, 3, 4, 5]
  // [2, 1] [7, 6]   [8, 3] [4, 5]
  // [2] [1] [7] [6] [8] [3] [4] [5]
```

Once we get down to an array with length one, the only thing left to occur is the merge operation at each level.  And that merge operation occurs from the bottom level up.  So it's really:
```javascript
merge([2], [1]) merge([7],[6])  merge([8], [3])  merge([4], [5])
// this merge operation combines the two arrays into an ordered array
// at the higher level up
merge([1, 2], [6, 7]) merge([3, 8], [4, 5])

merge([1, 2, 6, 7], [3, 4, 5, 8])

// [1, 2, 3, 4, 5, 6, 7, 8]
```

Ok, so that's mergeSort.  It consists of splitting the array in half until the subarrays have length one.  And then when you have subarrays of length one, you combine them call the merge operation which returns increasingly larger subarrays that are now sorted, until you have your final sorted array.  

### Calculating big O

So the merge sort function is written like this:

```javascript

function mergeSort(array){
  if(array.length < 2){
    return array
  } else {
    let midpoint = array.length/2
    let firstHalf = array.slice(0, midpoint)
    let secondHalf = array.slice(midpoint, array.length)
    merge(mergeSort(firstHalf), mergeSort(secondHalf))
  }
}
```
And we can think of it as doing the following:

```javascript
let array =  [1, 2, 6, 7, 8, 3, 4, 5, 9, 10]
mergeSort(array)

// 1. Splitting up
// [2, 1, 7, 6]      [8, 3, 4, 5]
// [2, 1] [7, 6]   [8, 3] [4, 5]
// [2] [1] [7] [6] [8] [3] [4] [5]

// 2. Merge
// merge([2], [1]) merge([7],[6])  merge([8], [3])  merge([4], [5])
// merge([1, 2], [6, 7]) merge([3, 8], [4, 5])
// merge([1, 2, 6, 7], [3, 4, 5, 8])
```

So how costly is something like this.  Well let's calculate the merging section first.  Notice that for an array of length 8, our merge section has three levels.  So notice that it's three levels because we split our array in half until the length is one.  So how many times do you have to split an array in half until the length is one?  log(n).  

> We said that the definition of log(n) is the number of times you have to press divide by 2 on a calculator until you get to one.  

Ok so the height of this merge section is log(n).  Now what is the cost at our first level.  
```javascript
// merge([2], [1]) merge([7],[6])  merge([8], [3])  merge([4], [5])
```

Well remember we said that the cost of merge is `firstHalf.length + secondHalf.length`.  So at the first level that merge operation occurs four times for a cost of two each.  At the second level merge occurs twice for a cost of four each, and at the final level merge happens once for a cost of eight.  In other words, what is the cost at each level, n.  

```javascript

// merge([2], [1]) merge([7],[6])  merge([8], [3])  merge([4], [5])
  2 + 2 + 2 + 2 = 8
// merge([1, 2], [6, 7]) merge([3, 8], [4, 5])
  4 + 4 = 8
// merge([1, 2, 6, 7], [3, 4, 5, 8])
  8
```

So each level costs n, and how much levels are there?  There are log n levels.  So the cost of merging is n*log n.  Now we still didn't cover the cost of splitting.  Let's look at the splitting stage again.

```javascript

let array =  [1, 2, 6, 7, 8, 3, 4, 5, 9, 10]
mergeSort(array)

// 1. Splitting up
// [2, 1, 7, 6]      [8, 3, 4, 5]
// [2, 1] [7, 6]   [8, 3] [4, 5]
// [2] [1] [7] [6] [8] [3] [4] [5]

```

So splitting occurs three times, and splitting an array in half does not cost n.  It costs the same regardless of the size of the array.  How many times does the splitting operation occur?  log n times.  This is because we continue to split the array in half until the array's length is one. So the cost of splitting is log n.  

So we can say that the big o of merge sort is the total cost of splitting plus the total cost of merging or log n + n log n.  Now in big o we only consider the largest exponent, and because n * log n is larger than log n the big o is n log n.

### So what?

Remember we started out the lesson with trying to improve upon selection sort.  Remember that selection sort cost us n^2.  Well in this example of sorting an array of eight elements n log n = 24 while n^2 = 64.  If n = 1000 then n^2 = 1,000,000 while n log n = 9,965.  So when is one thousand selection sort takes over a hundred times longer than merge sort.  That's significant.

Going forward, you can assume that the cost of sorting an array is n log n.  And now you know the steps involved in sorting an array.

### Summary

Merge sort involves two large components, recursively splitting up the array into smaller and smaller subarrays until the size of the array is one or smaller, and the merge step of looking at the first element of each subarray to find the min of both of them.  Mergesort can be expressed with the following pseudocode:

```javascript
  function mergeSort(array){
    if(array.length < 2){
      return array;
    } else {
      merge(mergeSort(firstHalfArray), mergeSort(secondHalfArray))
    }
    return sorted.concat(firstHalf).concat(secondHalf)
  }
```

Using our old recursive trick of looking of defining a solution in terms of the name of the function we see that a merge-sorted array is the same as merging two subarrays which are both merge-sorted. 
