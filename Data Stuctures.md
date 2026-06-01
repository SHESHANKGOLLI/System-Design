Real Time Example:
Stack = Google Chrome Browser(clicking on back button last visited will come)
Queue =  Movie Counter or online movie
Single LinkedList = Music Player (only one way)
					Song1 --> song2 --> song3
Double LinkedList = Music Player (Bidirectional way)
					Song1 --> song2 --> song3
						  <--		<--
Circular LinkedList = Music Player (Circular way) 
					
					Song1 --> song2 --> song3 a
						  <--		<--
===================================================================================
Data Structure
The data structure is a way that specifies how to organize and manipulate the data.
 
Linear Data Structure: 
A data structure is called linear if all of its elements are arranged in the sequential order. In linear data structures
 the elements are stored in a non-hierarchical way where each item has the successors and predecessors except the first and last element.
Arrays,LinkedList,Queue,Stack
Non-Linear Data Structure: 
The Non-linear data structure does not form a sequence i.e. each item or element is connected with two or more other items in a non-linear arrangement. 
The data elements are not arranged in the sequential structure.
Trees, Searching, Sorting,Graphs
=====================================================================================================================================================================================
Stack is an ADT (Abstract data type)  which uses arrays or linked list DS for the impl.
An ADT tells what is to be done and data structure tells how it is to be done. We can say that ADT gives us the blueprint while data structure provides the implementation part.
For example, the Stack ADT can be Impld  by both Arrays and linked list. Suppose the array is providing time efficiency while the linked list is providing space efficiency, Based on  the current user's requirements will be selected.
Advantages:  Efficiency , Reusability , Abstraction

Non Linear DS:
Stack: Push Pop Peek,Traverse  ( top = -1,  , stacksize=50)
Queue: Enqueue, Dequeue (rearend =  -1, frontend=1, queue size =50)


===================================================================================
Searching:
Linear Search: Just comparing the searchKey with array.
BinarySearch: We will sort the array first , middleTerm > key (lowerpart), middleTerm <key(higher part).. same  we wil repeat
                         Min== max=len-1, guess=min+max/2 and target=50(any number that u wat to search)
	    	Note: Faster than lnear.
===================================================================================
Sorting:
Bubble Sort: Comparing each element with adjacent element then sort if they are greater.
		   More time complexity O(n power 2 )  (Order of n square)
Selection Sort:
1)	In this algorithm we have to find the minimum value in the list first. Then, Swap it with the value in the first, then repeat the same
(int minValue = arr[0], int minIndex= 0;
			More time complixity
Insertion Sort:
1)	In this algorithm, We will insert particular element in its lower index of sublist if it appropriate location.
  


Quick Sort: O(n log(n))
	Ex: 10, 2, 9, 1 , 11,15,16.  Pivot = 10.
	https://www.youtube.com/watch?v=QN9hnmAgmOc
1)	In this algorithm, Consider any element in array as a pivot element.
2)	Partion array in two parts one part of array is greater than pivot and another is lesser than pivot.
Note: If you notice, pivot element is sorted and we will not touch that element.
2,9,1, 10, 11,15,16  (pivot element is sorted).
Same will be repeated.
3)	Now again devide part1 array with anyone as pivot element and same part 2 array.
4)	  

Merge Sort: O(n log(n))
	https://www.youtube.com/watch?v=jlHkDBEumP0
	1)First find the mid position of the array. 
		[12(0),13(1),14(2),15(3)] mid = len-1/2 = 1(index value)
		[12(0),13(1),14(2),15(3), 2(4)] mid = len-1/2 = 2(index value)
===================================================================================        
Time Complexity:
				Best		Average		Worst	
Selection Sort	Ω(n^2)		θ(n^2)		O(n^2)	 
Bubble Sort		Ω(n)		θ(n^2)		O(n^2)	 
Insertion Sort	Ω(n)		θ(n^2)		O(n^2)	 
Heap Sort		Ω(n log(n))	θ(n log(n))	O(n log(n))	 
Quick Sort		Ω(n log(n))	θ(n log(n))	O(n^2)	 
Merge Sort		Ω(n log(n))	θ(n log(n))	O(n log(n))	 
Bucket Sort		Ω(n+k)		θ(n+k)		O(n^2)	 
Radix Sort		Ω(nk)		θ(nk)		O(nk)
==========================================================================
Linear DataStruc:
Trees:

1)A tree consists of atmost 2 nodes and atleast no node


				Balanced Tree:  insert O(log n) , find O(log n)
					8
				  5	  9
				4 		10(balanced)
				UbBalanced Tree:  insert O(n) , find O(n)
				1
				 2
				  3
				   5 (unbalanced)
		======================================================================
Types of binary tree:
				1)Full/proper/Strict BT
						a)It is a BT, each node contain 2 children except leaf nodes.
						b)Number leaf nodes in FBT = Number of internal nodes +1
					 a		     a		   a	             a
				   a   a	   a	a	 a	 a      	  a      a
				  a a a a    a	a   	a a    		     a a    a a 
							a		 	        	    a      a     
					(Yes)		(No)   (yes - all LN		     (No)
									   dont have childs)
				==========================================================================
				2)Complete
						a)If all the level are completely filled and except that last level should be filled from left side.
					 a		     a			          a
				   a   a	   a	a		      a      a
				  a a a a    a	a  a  a 	     a a    a a 
							a		 	        a      a     
					(Yes)		(Yes)				(Not)
				==========================================================================
				3)Perfect
						a)All the internal should contain two childresn
						b)ALl the leaf nodes should be at same level.
						c) And also it is said to be CBT and FBT if it is PBT
						
					  a		     a			          a
				   a   a	   a	a		      a      a
				  a a a a    a	a  a  a 	     a a    a a 
							a		 	       a      a     
					(yes)        (No)                 (No)
				==========================================================================
				4)Degenerate
						a)All internal should have only the one child either left or right.
						b)It is also called as LeftSquedBT OR RightSquedBT or DegenerateBT
					a			a			 	 a
				   a 			 a	   			a
				  a				  a			 	 a
				 a		  		   a   			a
				(LeftSquedBT)  (RightSquedBT)  
				==========================================================================
				5)Balanced binary tree
						Balance Factor (k) = height (left(k)) - height (right(k))
						a)It is also called as RedBlackTree and AVL tree
							LL, RR, LR, RL
	==================================================================================
Tree Traversal:

1)Breadth First Traversal  (level order)

		a
	  b    h
	 e f  c g
 (a b h e f c g)
2)Depth First Traversal

	Preporder 					Inorder		  				PostOrder;
	Root Left Right				Left Root Rigt			   	Left Right Root
	 10				 			 10							 10
	8 11						8 11  						8 11 
  (10,8,11)					  (8,10,11)					   (8,11,10)
===================================================================================================================
Graphs











