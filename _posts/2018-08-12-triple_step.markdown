---
layout: post
title:      "Triple Step"
date:       2018-08-12 22:15:18 +0000
permalink:  triple_step
---


I recently went to an interview where they asked me the Triple Step (Using 1,2,4 instead of 1,2,3) problem. They were pretty surprised with the answer I came up with since most of the time its solved recursively. So I thought I'd make a quick blog post about it.

If you don't know the the triple step quesiton its: 
A child is running up a staircase with n steps and can hop either 1 step, 2 steps, or 3 steps at a time. Implemenet a method to count how many possible ways the child can run up th stairs.

At first glance, its a recursive question. 5 steps is 4 steps+3 steps+2 steps, etc. etc.
So your typical answer would look like this: 
```
let triple_step_rec = function(num, result){
    if(num < 0){
        return 0
    }

    if(result[num] > 0){
        return result[num];
    }

    result[num] = triple_step_rec(num-1, result) +
                  triple_step_rec(num-2, result) +
                  triple_step_rec(num-3, result);
    return result[num];
}

let triple_step = function(num){
    if(num <= 0){
        return 0;
    }
    let result = [];
    for(let i = 0; i < num+1; i++){
        result[i] = 0;
    }

    result[0] = 1;

    triple_step_rec(num, result);

    return result[num]
}

```

This one requires some space complexity of O(n).

My solution was constant time interms of space complexity and doesn't rely on recursion which, personally, always messes me up.

```
let triple_step = function(num){
    if(num <= 0){
        return 0
    }
    let current_sum = 0;
    let result = [0,0,0,1];

    for(let i = 0; i < num; i++){
        current_sum = result[1] + result[2] + result[3];
        result[0] = result[1]
        result[1] = result[2]
        result[2] = result[3]
        result[3] = current_sum
    }
    return result[3]
}
```
