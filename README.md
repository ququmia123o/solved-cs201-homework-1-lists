Download Link: https://assignmentchef.com/product/solved-cs201-homework-1-lists
<br>
In this assignment, we will implement 2 different data structures to represent a list. We will use the list to implement an image and we will write operations on those images. We will then time the operations in order to plot and compare the relative performance of the backing data structures.

1       Image Operations

We will work with RGB images and perform 3 operations on them–channel suppression, rotations, and mask application. None of these operations is <em>destructive</em>. That is, the operations do not alter the original image, rather they return a new image containing the result of the operation.

1.1      Channel Suppression

An image is said to contain color values in different <em>channels</em>. In an RGB image, the channels are red, blue, and green. Each channel contains the intensities for that color for every pixel in the image. The values from all 3 channels at a pixel yields the RGB value at the pixel. The channel suppression operations switches off specifies channels. That is, all intensities in that channel will be turned to 0, or turned off. Figure 1 shows an original image and 2 modifications, once with the blue channel turned off, and then with only the blue channel turned on, i.e. the red and green channels turned off.

1.2     Rotation

Given a square image, i.e. one whose width is equal to its height, this operation generates a new image that contains rotations of the original image. Figure 2 shows an example of applying the operation. The resulting image has twice the dimensions of the original image, i.e. twice the width and twice the height.

(a) An RGB image of a swimming (b) The image with the blue chan- (c) The original image with only pool. nel turned off. the blue channel turned on.

Figure 1: Example of channel suppression.

1

<ul>

 <li>A square image.</li>

 <li>The image obtained as a result of applying rotations to the original image.</li>

</ul>

Figure 2: Example of rotation.

<h2>     1.3      Applying a Mask</h2>

A mask specifies certain <em>weights </em>and applying the mask to an image entails replacing the value of each pixel in the image with a <em>weighted average </em>of the values of its <em>neighbors</em>. The weights and the neighbors to consider for the average are specified by the mask.

The mask is an <em>n </em>× <em>n </em>array of non-negative integers representing weights. For our purposes, <em>n </em>must be odd. This means that the <em>n </em>× <em>n </em>array has a well defined center–the <em>origin</em>. The weights in the mask can be arbitrary integers–positive, negative, or zero.

For each pixel in the input image, think of the mask as being placed on top of the image so its origin is on the pixel we wish to examine. The intensity value of each pixel under the mask is multiplied by the corresponding value in the mask that covers it. These products are added together. Always use the original values for each pixel for each mask calculation, not the new values you compute as you process the image. For example, refer to Figure 3a, which shows the 3 × 3 mask,

and an image on which we want to perform the mask computation. Suppose we want to compute the result of the mask computation for pixel <em>e</em>. This result would be:

<em>a </em>+ 3<em>b </em>+ <em>c </em>+ 3<em>d </em>+ 5<em>e </em>+ 3<em>f </em>+ <em>g </em>+ 3<em>h </em>+ <em>i</em>

Some masks require a <em>weighted average </em>instead of a weighted sum. The weighted average in the case of Figure 3a for pixel <em>e </em>would be:

Instead of doing this calculation for each channel individually, use the average value of the red, green, and blue channels. For example, if the pixel is given by (<em>r,g,b</em>) = (107<em>,</em>9<em>,</em>218), then use (107+9+218)<em>//</em>3 = 111 for each channel. This effectively converts the image to grayscale.

Note that sometimes when you center the mask over a pixel you want to operate on, the mask will hang over the edge of the image. In this case, compute the weighted sum of only those pixels the mask covers. For the example shown in Figure 3b, the result for the pixel <em>e </em>is given by:

3<em>b </em>+ <em>c </em>+ 5<em>e </em>+ 3<em>f </em>+ 3<em>h </em>+ <em>i</em>

If no average is required and the following if an average is required:

(b) If the mask hangs over the edge of the image, use only

(a) Overlay the 3×3 mask over the image so it is centered those mask values that cover the image in the weighted

on pixel <em>e </em>to compute the new value for pixel <em>e</em>.

sum.

Figure 3: Applying a mask to an image.

<h3>     1.3.1        Applications of Masks</h3>

Applying different masks leads to different properties. For example, applying the following mask above leads to blurring of the image. Figures 4a and 4b show the blurring effect of this mask. Note that color information is lost as mentioned above.

<table width="137">

 <tbody>

  <tr>

   <td width="35"> 2<sub> </sub>4 5  42</td>

   <td width="27">491294</td>

   <td width="27">51215125</td>

   <td width="27">491294</td>

   <td width="22">2 4   <sub></sub>5   <sub></sub>4 <sub></sub>2</td>

  </tr>

 </tbody>

</table>

Another application we use in an implementation of <em>Canny Edge Detection </em>using <em>Sobel Operators</em>. Once the image has been blurred as above, two more <em>filters</em>, or masks, (the Sobel operators) are applied in succession to the blurred image. These filters determine the change in intensity, which approximates the horizontal and vertical derivatives.

. After these operations are applied one after the other to the blurred image, the values obtained are used to search for edges based on the magnitude and direction of the change in intensity. An example of the final result is shown in Figure 4c.

<h1>     2      Image</h1>

We treat an image as a grid of <em>pixels </em>where each pixel has an RGB value indicating the red, green, and blue intensities of the pixel. An image has <em>dimensions</em>, namely <em>width </em>and <em>height</em>, which determine the number of <em>rows </em>and <em>columns </em>in the image. Every pixel in the image is at a unique combination of row and column numbers which can therefore be used as coordinate system in the image. An image with width <em>w </em>and height <em>h </em>is said to be of size <em>w </em>× <em>h</em>. Figure 5a shows the column and row numbers in a <em>w </em>× <em>h </em>image along with the resulting pixel coordinates. Note that the coordinate is just a means to locate a pixel in the image, it is not the value stored at the pixel. The value stored at a pixel a triplet denoting the red, green, and blue intensities respectively.

We will work with a <em>flattened</em>representation of an image. That is, we will store the pixel values in a 1dimensional list structure as opposed to 2-dimensional structure. The list stores pixel values as they appear in the image from left to right and top to bottom. Figure 5b shows a 5×5 image with some supposed RGB

(a) An image with sharp details and (b) Result of applying the blur mask (c) Result of applying the Sobel filseveral lines. to the original image. ters to the blurred image.

Figure 4: Blurring and detection of edges in an image using masks.

Columns

<table width="111">

 <tbody>

  <tr>

   <td width="22"><em>a</em></td>

   <td width="21"><em>b</em></td>

   <td width="25"><em>c</em></td>

   <td width="22"><em>d</em></td>

   <td width="21"><em>e</em></td>

  </tr>

  <tr>

   <td width="22"><em>f</em></td>

   <td width="21"><em>g</em></td>

   <td width="25"><em>h</em></td>

   <td width="22"><em>i</em></td>

   <td width="21"><em>j</em></td>

  </tr>

  <tr>

   <td width="22"><em>k</em></td>

   <td width="21"><em>l</em></td>

   <td width="25"><em>m</em></td>

   <td width="22"><em>n</em></td>

   <td width="21"><em>o</em></td>

  </tr>

  <tr>

   <td width="22"><em>p</em></td>

   <td width="21"><em>q</em></td>

   <td width="25"><em>r</em></td>

   <td width="22"><em>s</em></td>

   <td width="21"><em>t</em></td>

  </tr>

  <tr>

   <td width="22"><em>u</em></td>

   <td width="21"><em>v</em></td>

   <td width="25"><em>w</em></td>

   <td width="22"><em>x</em></td>

   <td width="21"><em>y</em></td>

  </tr>

 </tbody>

</table>

<table width="320">

 <tbody>

  <tr>

   <td width="66">(0<em>,</em>0)</td>

   <td width="66">(0<em>,</em>1)</td>

   <td width="66">(0<em>,</em>2)</td>

   <td width="32"><em>…</em></td>

   <td width="90">(0<em>,w </em>− 1)</td>

  </tr>

  <tr>

   <td width="66">(1<em>,</em>0)</td>

   <td width="66">(1<em>,</em>1)</td>

   <td width="66">(1<em>,</em>2)</td>

   <td width="32"><em>…</em></td>

   <td width="90">(1<em>,w </em>− 1)</td>

  </tr>

  <tr>

   <td width="66">(2<em>,</em>0)</td>

   <td width="66">(2<em>,</em>1)</td>

   <td width="66">(2<em>,</em>2)</td>

   <td width="32"><em>…</em></td>

   <td width="90">(2<em>,w </em>− 1)</td>

  </tr>

  <tr>

   <td width="66">...</td>

   <td width="66">...</td>

   <td width="66">...</td>

   <td width="32">…</td>

   <td width="90">...</td>

  </tr>

  <tr>

   <td width="66">(<em>h </em>− 1<em>,</em>0)</td>

   <td width="66">(<em>h </em>− 1<em>,</em>1)</td>

   <td width="66">(<em>h </em>− 1<em>,</em>2)</td>

   <td width="32"><em>…</em></td>

   <td width="90">(<em>h </em>− 1<em>,w </em>− 1)</td>

  </tr>

 </tbody>

</table>

0                    1                    2              …             (<em>w </em>− 1)

0

1

2

.

.

.

(<em>h </em>− 1)

(a) Row and column numbers of an image with width <em>w </em>and height <em>h</em>. Pixel (b) A 5 × 5 image with supposed coordinates are also shown.              pixel values.

Figure 5: Image dimensions and pixel coordinates.

values. Note that each value would be a triplet of integers, each integer between 0 and 255 inclusive. Using our representation, the image in Figure 5b will be represented as the list:

[<em>a,b,c,d,e,f,g,h,i,j,k,l,,m,n,o,p,q,r,s,t,u,v,w,x,y</em>]

<h1>     3        Implementation Details and Tasks</h1>

<h2>     3.1     Image Operations</h2>

You have to implement the image operations shown in Listing 1 on Page 6 and also included in the accompanying file image operations.py. These correspond to those illustrated in Section 1. The operations manipulate an instance of our image type, called MyImage. Note that none of the operations is destructive.

That is, the original image is not modified. The result of an operation is returned as a new MyImage instance.

<h2>     3.2     Image</h2>

We implement our custom image type as MyImage as shown in Listing 2 on Page 7 and also included in the accompanying file myimage.py. Note that the implementation uses on MyList which is our implementation of the list. The given implementation is complete and you are not to change it except for the indicated line in MyImage. init (). You will change this line to use your implementation of MyList. This is the only change you are allowed to make in the implementation of MyImage.

<h2>     3.3     List</h2>

We implement our custom list as MyList whose interface shown in Listing 3 on Page 9 and also included in the accompanying file mylist.py. Note that the implementation is mostly complete except for the segments marked as pass. The implementation includes a mechanism to allow iteration over the list and using some other native python syntax with it.

You have to copy the implementation to ArrayList and PointerList which will implement the list using <a href="https://www.programiz.com/python-programming/array">python arrays</a> and pointers respectively. See the implementation of BST in Listing 4 on Page 11 and also included in the accompanying file bst.py to understand pointers in python. You then have to modify the required line in myimage.py to use your type.

<h1>     4      Timing</h1>

We want to perform a timing analysis on the execution of the image operations using our list implementations. Those details are forthcoming.

<h1>     5      Bonus</h1>

You can earn bonus marks for implementing interesting image manipulation algorithms of your own. Please see the rubric for more details. Interesting submissions will be featured on the course’s forthcoming web page.

<h1></h1>