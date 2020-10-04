## Merge Sort Algorithm

### Background

So in the last lesson, we saw a function called merging that allowed us to merge two sorted subarrays into one combined array.  In this lesson, we will use that function when learning about the merge sort algorithm.

### Merge Sort in a nutshell

The merge sort algorithm sorts a list in two main steps.  

**First**, take an unordered list and continue splitting the list into smaller and smaller subarrays until the subarrays each one element.

```javascript
[7, 1, 2, 8, 6, 10, 4, 3]
  
[7, 1, 2, 8]   [6, 10, 4, 3]

[7, 1] [2, 8]   [6, 10] [4, 3]

[7] [1] [2] [8]  [6] [10] [4] [3]
```

So the first step is to take the unsorted list and break it up.

**Second**, couple up the neighboring lists and merge them together.  While the merge operation only applies to sorted lists, when the list has only one element in it, it is automatically sorted.

```javascript
merge([7],[1]) merge([2], [8])  merge([6], [10]) merge([4], [3])
```
Now think about what this gives us.  Well the merge operation takes two sorted arrays and combines them into one sorted array.  So merging pairs of eight sorted arrays, produces four sorted arrays, which we can then merge again.

```javascript
merge([7],[1]) merge([2], [8])  merge([6], [10]) merge([4], [3])

// [1, 7] [2, 8] [6, 10] [3, 4] 

merge([1, 7], [2, 8]) merge([6, 10] [3, 4])

// [1, 2, 7, 8]  [3, 4, 6, 10] 
// which we can merge again
merge([1, 2, 7, 8], [3, 4, 6, 10])

//[1, 2, 3, 4, 6, 7, 8, 10]
// providing us with a sorted array
```

That's it!  Two steps.  First, continuously divide an *unsorted array* until we have subarrays each of one element.  Second, call the merge operation on the subarrays until we are left with one *sorted array*. 

Try to place this in your own words, before moving on.  

Question: How does mergesort work?  
Answer: "Well we start with..."

### Coding Merge Sort

So we said that Merge Sort begins by dividing up an array.  Let's *simplify the problem* by starting with an unsorted array of length 2. 

```javascript
	let unsortedArray = [2, 1]
	
	function mergeSort(){
	}
```

**Step one** is to split the array in two.   

```javascript
let unsortedArray = [2, 1]
	
function mergeSort(array){
  let midpoint = array.length/2
  let firstHalf = array.slice(0, midpoint)
  let secondHalf = array.slice(midpoint, array.length)
}

```
Now, note that `firstHalf` now equals `[2]` and `secondHalf` equals `[1]`.  What's left? **Step two** is to merge this array together.

```javascript
let unsortedArray = [2, 1]
	
function mergeSort(array){
  let midpoint = array.length/2
  let firstHalf = array.slice(0, midpoint)
  let secondHalf = array.slice(midpoint, array.length)
  return merge(firstHalf, secondHalf)
}

mergeSort(unsortedArray)
// [1, 2]

```

Now that we have this working, the next step is to turn this into a recursive solution that will work for arrays of any length.  So we say that when looking for the recursive solution, we try to word the solution in terms of itself.  

Here, we can do just that.  In the line `merge(firstHalf, secondHalf)` we can replace `firstHalf` and `secondHalf` with `mergeSort(firstHalf)` and `mergeSort(secondHalf)`, because `mergeSort` just returns a sorted array.  So now our function looks like the following: 

```javascript
function mergeSort(array){
  let midpoint = array.length/2
  let firstHalf = array.slice(0, midpoint)
  let secondHalf = array.slice(midpoint, array.length)
  return merge(mergeSort(firstHalf), mergeSort(secondHalf))
}

mergeSort(unsortedArray)
```

Finally, we find our base case, which is that when our array has only one element in it, we simply return the array.  So finally, our finished `mergeSort` function looks like the following: 

```javascript
  function mergeSort(array){
    let midpoint = array.length/2
    let firstHalf = array.slice(0, midpoint)
    let secondHalf = array.slice(midpoint, array.length)
    
    if(array.length < 2){
      return array
    } else {
      return merge(mergeSort(firstHalf), mergeSort(secondHalf))
    }
  }
```

### Really?
Let's walk through why this works.  If the array is just one element, mergeSort returns that array.  (An array of length one is automatically sorted).

So now let's try to understand the second half of the function.  So the first thing mergeSort does is split the array in halves.

```javascript

mergeSort([7, 8, 1, 6])

function mergeSort(array){
	let midpoint = array.length/2
	let firstHalf = array.slice(0, midpoint)
	let secondHalf = array.slice(midpoint, array.length)
	    
	if(array.length < 2){
	  return array
	} else {
	  merge(mergeSort(firstHalf), mergeSort(secondHalf))
	}
  }
```
And it continues to split the array in half, because each time before ever hitting the merge function, we pass the subarray to mergeSort which immediately splits the array in two again.  When does this stop?  When the array is of length one.  

```javascript
  let array =  [2, 1, 7, 6, 8, 3, 4, 5]

  mergeSort(array)
  // [2, 1, 7, 6]      [8, 3, 4, 5]
  // [2, 1] [7, 6]   [8, 3] [4, 5]
  // [2] [1] [7] [6] [8] [3] [4] [5]
```

Once we get down to an array with length one, then mergeSort of an array of length one just returns the same array, it can begin to merge the subarrays together.  So the rest of the procedure looks like the following.

```javascript
merge([2], [1]) merge([7],[6])  merge([8], [3])  merge([4], [5])
// this merge operation combines the two 
// arrays into an ordered array
// at the higher level up
merge([1, 2], [6, 7]) merge([3, 8], [4, 5])

merge([1, 2, 6, 7], [3, 4, 5, 8])

// [1, 2, 3, 4, 5, 6, 7, 8]
```


### Summary 
Ok, so that's mergeSort.  It has a nice recursive solution that can be expressed with the following pseudocode: 

```javascript
  function mergeSort(array){
    if(array.length < 2){
      return array;
    } else {
      merge(mergeSort(firstHalfArray), mergeSort(secondHalfArray))
    }
  }
```

It consists of splitting the array in half until the subarrays have length one.  And then when you have subarrays of length one, you combine them call the merge operation which returns increasingly larger subarrays that are now sorted, until you have your final sorted array. 


<p class='util--hide'>View <a href='https://learn.co/lessons/merge-sort-codealong'>Merge Sort Codealong</a> on Learn.co and start learning to code for free.</p>
