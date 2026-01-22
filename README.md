# Minimum-Pair-Removal-to-Sort-Array-I

Given an array nums, you can perform the following operation any number of times:

Select the adjacent pair with the minimum sum in nums. If multiple such pairs exist, choose the leftmost one.
Replace the pair with their sum.
Return the minimum number of operations needed to make the array non-decreasing.

An array is said to be non-decreasing if each element is greater than or equal to its previous element (if it exists).

 

Example 1:

Input: nums = [5,2,3,1]

Output: 2

Explanation:

The pair (3,1) has the minimum sum of 4. After replacement, nums = [5,2,4].
The pair (2,4) has the minimum sum of 6. After replacement, nums = [5,6].
The array nums became non-decreasing in two operations.

Example 2:

Input: nums = [1,2,2]

Output: 0

Explanation:

The array nums is already sorted.

class Solution:
    def minimumPairRemoval(self, a: List[int]) -> int:
        SUMM,I,V1,V2,LEFT,RIGHT = 0,1,2,3,4,5
        
        b = [[v+u,i,v,u,None,None] for (v,u),i in zip(pairwise(a),count())]
        if len(b)>1: b[0][RIGHT],b[-1][LEFT] = b[1],b[-2]
        for l,m,r in zip(b,b[1:],b[2:]):
            l[RIGHT],m[LEFT],m[RIGHT],r[LEFT] = m,l,r,m

        z,sl,res = sum(starmap(gt,pairwise(a))),SortedList(b),0
        while z:
            summ,_,v,u,l,r = sl.pop(0)
            z,res = z-(v>u),res+1
            if l:
                sl.remove(l)
                z = z-(l[V1]>l[V2])+(l[V1]>summ)
                l[V2],l[SUMM],l[RIGHT] = summ,l[V1]+summ,r
                sl.add(l)
            if r:
                sl.remove(r)
                z = z-(r[V1]>r[V2])+(summ>r[V2])
                r[V1],r[SUMM],r[LEFT] = summ,summ+r[V2],l
                sl.add(r)
            
        return res

 

Constraints:

1 <= nums.length <= 50
-1000 <= nums[i] <= 1000
