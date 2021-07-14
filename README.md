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


**Boundary threshold**

<img src="img/eq12.png" >





## Code details



Overall flow：
Input imgae -> gaussian filter ->  predicate



