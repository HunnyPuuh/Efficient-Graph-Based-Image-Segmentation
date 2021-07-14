# Efficient-Graph-Based-Image-Segmentation
This section is to replicate the algorithm proposed by Felzenszwalb, P.F. and Huttenlocher, D.P.(Efficient graph-based image segmentation) by **python**

You can find the paper via http://cs.brown.edu/people/pfelzens/papers/seg-ijcv.pdf 

The authors also implement the alorithm in C++ and you can find it via http://cs.brown.edu/people/pfelzens/segment/

## Highlight
1. The algorithm can capture important **non-local properties**
2. The algorithm is computationally efficient - running in O(nlogn) for n image pixels


## Paper knowledge


### Non-local image characteristic


<img src="img/synthetic_foto.png" >
 
Most people will say that this image(shown above) has **three distinct regions**: 
- a rectangularshaped intensity ramp in the left half **(Region 1)**
- a constant intensity region with a hole on the right half **(Region 2)**
- a high-variability rectangular region inside the constant region **(Region 3)**


So above image illustrates some perceptually **important properties** that should be captured by a segmentation algorithm.
1. Widely varying intensities should not alone be judged as evidence for multiple regions. For example,such wide variation in intensities occurs both
  in the ramp on the left **(Region 1)** and in the high variability region on the right **(Region 3)**. Thus it is not adequate to assume
  that regions have nearly constant or slowly varying intensities.
2.  Three regions **(Region 1,2,3)** cannot be obtained using purely local decision criteria. This is because the intensity difference across the boundary **(Region 1 and 2)**    between the ramp and the constant region is actually smaller than many of the intensity differences within the high variability region **(Region 3)**. Thus, in order to          segment such an image, some kind of adaptive or non-local criterion must be used.


Wrong segmentation

<img src="img/wrong_segmentation1.png" >  <img src="img/wrong_segmentation2.png" >

Correct segmentation

<img src="img/correct_segmentation.png" >

### Graph-based segmentation


<img src="img/eq1.png" > represents an undirected graph with vertices and edges

In the image segmentation,

For vertices <img src="img/eq2.png" >,  they are pixels

For egdes <img src="img/eq3.png" >, they are pairs of neighboring vertices. 
Each edge has a corresponding weight <img src="img/eq4.png" > , which is non-negative measure of dissimilarity between neighboring <img src="img/eq5.png" > and <img src="img/eq6.png" >. 
In terms of definition of weight function, it will be discussed later

A segmentation *S* is a partition of vertices into components such that each components(or region) <img src="img/eq7.png" > corresponds to a connected component in a graph
<img src="img/eq8.png" > ,where <img src="img/eq9.png" >

Note: You can think S includes all components.Within each component, it has connections between vertices. One segmentation is different from other segmentation by how it divides the vertices




### Pairwise Region Comparison Predicate
This section defines a predicate for evaluating whether or not there is evidence for a boudary between two components in a segmentation(two regions of an image)


**Internal difference**:it measures difference within one component and it is the largest weight in the minimum spanning tree of the component
<img src="img/eq10.png" >

In the extreme case, if one component contains only one vertice(i.e single pixel),when |C|=0, Int(C)=0


**External difference**: it measures difference between two components and it is the minimum weight edge conneting two components
<img src="img/eq11.png" >

If there is no edge connneting C1 and C2, then  <img src="img/eq12.png" >

Based on two definitions above, we further define **Boundary threshold**

**Boundary threshold**

<img src="img/eq13.png" >

Where the minimum internal difference,MInt, is defined as 


<img src="img/eq14.png" >


In summary, the overall idea is if external difference is larger than the internal difference, then boudary can be formed

Note: The threshold function τ controls the degree to which external difference must be greater than their internal differences 
in order for there to be evidence of a boundary between them (D to be true).

The reason why we have this threshold function τ is explained as follows:
For small components(which means component includes few vertices), using Int(C) is not a good estimate of the local characteristics of the data
In the extreme case,when |C| = 1, Int(C) = 0. Then the internal difference is samll and helps to form the boundary and may lead to multiple components, which may in essence
can be included in one component.

Therefore, we use a threshold function based on the size of the component, τ (C) = k/|C|, where |C| denotes the size of C, and k is some constant parameter(hyperparameter)
That is, for small components we require stronger evidence for a boundary. A larger k causes a preference for larger components. Note, however, that k is not a minimum component size. Smaller components are allowed when there is a sufficiently large difference between neighboring components.

## Algorithm and its properties
First, introduce the algorithm process 

Input: <img src="img/eq5.png" > with n vertices and m edges

Output: a segmentation of V into components <img src="img/eq16.png" >

Process:

1. Sort E into π=(O<sub>1</sub>,...O<sub>m</sub>) by non-decreasing edge weight.
2. Start with a segmentation S<sup>0</sup>, where each vertex v<sub>i</sub> is in its own component. (Initialization)

(For clarity, we notice there is superscript of 0 on S. Because the whole process is iterative, the superscript indicates the ith updates of S after considering ith edges)

<img src="img/eq5.png" >

3. Repeat step 4 for q=1,2,...m, which means considering each edge 
4. Construct S<sup>q</sup> given S<sup>q-1</sup> as follows.

Let v

Overall flow：
Input imgae -> gaussian filter ->  predicate



