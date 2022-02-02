## Assignment 03: EM for Gaussian Mixture Models (GMMs): Image Segmentation Application  

##### HE Yongyi from USTC

For this problem, we will use Gaussian mixture models (GMMs) for image segmentation and study the behavior of the algorithm on a simple image. The file "GMMSegmentTestImage.jpg" available on the website provides an image that is used as a test for color blindness. Color normal individuals viewing the image should see a clear message in the image revealed by a partitioning of the image into distinct regions that are characterized by homogeneity of some color attributes. However, these regions also show variation in other attributes.
We will segment the image using Gaussian mixture modeling and assess the results we obtain. For the GMM fitting in this assignment, you may choose to use your own program using the equations provided during our discussion of GMMs in the context of the EM algorithm in class, or you may choose to use an existing implementation from the web, or from the appropriate MATLAB$^{\bf{TM}}$ toolbox or python package.  

(a) To simplify things and allow us to visualize our results, we will work in a 2-dimensional space instead of directly using the 3-dimensional RGB image data Convert the image RGB data from its native 3-dimensional RGB  color space into a 2-dimensional $rg$ chromaticity space by applying the transformation
$$
\begin{align}
r &=\frac{R}{R+G+B} \\
g &=\frac{G}{R+G+B} . \tag{1}
\end{align}
$$
Transform the test image into $rg$ chromaticity space and visualize your transformed image by mapping it back into an 8-bit $RGB$ image via the transformation  
$$
\begin{align}
\alpha&=\frac{255}{\max (r, g, 1-(r+g))} \\
R_{o u t}&=\text { round }(\alpha r) \\
G_{o u t}&=\text { round }(\alpha g) \\
B_{o u t}&=\text { round }(\alpha(1-(r+g))) .\tag{2}
\end{align}
$$
Comment on what attributes are preserved and what attributes are lost in the process of conversion from the input RGB to the output RGB (equivalently from RGB space to $rg$  chromaticity space.
**Hint**: If you are using MATLAB, the functions `imread()` and `imshow()`, will be helpful for you in this exercise.

**Solution**:

First, convert the image RGB data from its native 3-dimensional RGB  color space into a 2-dimensional $rg$ chromaticity space by applying the transformation (1).

The sum of $r,g,b$ will always be equal to 1. Knowing any two of these values, the third value can be computed. So we can look at the $rg$ chromaticity space vertically (i.e., discard attribute $b$) without losing any information.

```python
im = Image.open('GMMSegmentTestImage.jpg')
pix = im.load()
width = im.size[0]
height = im.size[1]
RGB = [[0] * width ] * height
rg = []
co = []
for x in range(height):
    for y in range(width):
        r, g, b = pix[x, y]
        RGB[x][y] = [r,g,b]
        rg.append([r/(r+b+g), g/(r+b+g)]) # convert into 2D rg chromaticity space.
        co.append((r/(r+b+g), g/(r+b+g),b/(r+b+g))) # RGB to rgb
```

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210728003731900.png" alt="image-20210728003731900" style="zoom:67%;" />

Then, in order to visualize the transformed image, we map back that into an 8-bit $RGB$ image.

```python
width = im.size[0]
height = im.size[1]
im_out = im.copy()
pix_out = im_out.load()
co_out = np.array(co).reshape((height,width,3))
for x in range(height):
    for y in range(width):
        alpha = 255/(t[x][y].max())	# map back into RGB color value
        r_out = round(alpha*t[x][y][0])
        g_out = round(alpha*t[x][y][1])
        b_out = round(alpha*t[x][y][2])
        pix_out[x, y] = (r_out,g_out,b_out)
im_out.show()
```

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210728014108592.png" alt="image-20210728014108592" style="zoom:67%;" />

In RGB space, pixels are identified by the intensity of the red, green and blue primary colors. Thus, a bright red can be represented as (255,0,0), and a dark red can be represented as (40,0,0). However, in the $rg$ chromaticity space, colors are represented by the ratio of red, green and blue in the color, not by the intensity of each color. Because the sum of these ratios must be 1, we can only refer to the red and green ratios to represent the colors and can calculate the blue values as needed. **So the ratio of red, green and blue is preserved and the intensity information is lost in the process of conversion from RGB space to $\bf{rg}$ chromaticity space.** More professionally, the irradiance information is lost.

-------------------



(b) Cluster the pixels in the image by fitting a $K = 3$  component GMM to the  chromaticities in the image. Specifically, model the observed $rg$ chromaticity values across the pixels in the image as a set of iid $d =$ 2-dimensional $rg$ vector observations from a Gaussian mixture with unknown mean and covariance and use the Expectation Maximization (EM) algorithm for GMM parameter estimation to estimate the unknown mean vectors and covariance matrices for the GMM. Because the EM algorithm is only locally convergent, you need to perform the GMM parameter estimation with multiple random initializations and select between these to obtain a "good" result. For determining a "good" result, you can either evaluate the likelihood function, or (more readily) you can visualize your results as described in the following parts and assess the fit based on the visualizations.
**Hint**: The MATLAB function `reshape()` will be helpful for re-organizing the data from the $2D$ two $rg$  channel  chromaticity image representation into a sequence of $2D$ $rg$ vectors in this exercise.  

1. Visualize the GMM fit you have obtained by making a scatter-plot of the $rg$ chromaticities for the pixels in the image and superimposing contours for the components in your estimated GMM on this plot. In particular, for ea h mixture component in the GMM, indicate mean value of the mixture component on the scatter-plot by a "$+$" and plot the "$3\sigma$" contour that corresponds to locations at which the components' probability falls to a level that is a fraction exp($-3^2/2$) = exp($-9/2$) $\approx0.0111$ of its peak value. You may find it useful to use the singular value decomposition (SVD) to obtain your contours - although for this 2D setting, you can also compute these analytically using the formula for the roots of a quadratic equation.
2. Use the GMM to compute, for each pixel, the posterior probability that it came from the mixture component $j$, for $j=1,2,...K$. Visualize the results of the "soft" or probabilistic segmentation you have obtained in this process as a series of images. Specifically, for each mixture component $j$,  create and display a normalized image that represents the posterior probability that a pixel belongs to the class $j$ as an 8-bit gray-scale value, where gray-scale values of 0 corresponds to a probability of 0 and a gray-scale value of 255 corresponds to a probability of 1. Present one set of $K$ such images for one of the "good" mixture model fits that you obtain. Comment on your results.
3. Comment on the effect of different initializations on your GMM fit and how these appear in the visualizations presented in the preceding two parts.



**Solution**：

First, make a scatter-plot of $rg$ chromaticity using the primary color of each pixel in the image. Then we can roughly determine the region of each category and use it for subsequent comparison of the results.

```python
a = np.array(rg).reshape(-1,2) # reshape rg space
plt.scatter(a[:,0],a[:,1],s=5,c=co) # 'co' is rgb color value list
plt.xlabel('r')
plt.ylabel('g')
plt.show()
```

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210728004104146.png" alt="image-20210728004104146" style="zoom: 40%;" />

For each value of $K$, we use three random initializations to compare the results. **The means in the three random initializations are different, and the covariances and weights are the same.**

-----------------

**Initialization Ⅰ**

means_1:<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210728224139136.png" alt="image-20210728224139136" style="zoom:67%;" />

Visualization results of the GMM fit:

```python
# Draw elliptical contours using mean values and covariances.
def draw_ellipse(position, covariance, ax=None, **kwargs):
    ax = ax or plt.gca()
    if covariance.shape == (2, 2):
        U, s, Vt = np.linalg.svd(covariance)
        angle = np.degrees(np.arctan2(U[1, 0], U[0, 0]))
        width, height = 2 * np.sqrt(s)
    else:
        angle = 0
        width, height = 2 * np.sqrt(covariance)
	#	3-sigma contour 
    for sigma in range(1, 4):
        ellipse = Ellipse(position, sigma * width, sigma * height,angle, **kwargs)
        ax.add_patch(ellipse)
        ellipse.set(fc='None',ls='-.',ec='blue',alpha=0.6,lw=1)

#	Visualize the GMM fit
def plot_gmm(gmm, X, label=True, ax=None):
    ax = ax or plt.gca()
    labels = gmm.fit(X).predict(X)
    ax.scatter(X[:, 0], X[:, 1], c=labels, s=3, cmap='viridis')
    ax.scatter(gmm.means_[:,0],gmm.means_[:,1],s=50,marker='+',c='r')
    plt.xlabel('r')
    plt.ylabel('g')
    w_factor = 0.2 / gmm.weights_.max()
    for pos, covar, w in zip(gmm.means_, gmm.covariances_, gmm.weights_):
        draw_ellipse(pos, covar, alpha=w * w_factor)
```

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210728224156078.png" alt="image-20210728224156078" style="zoom: 40%;" />

We can see that all three classifications in this initialization converge to valid values, and have a good classification result. Then create and display normalized images for each category based on posterior probabilities.

```python
def norm(im,j,p=post_p):
    # 'post_p' is the list of posterior probabilities fitted by GMM.
    pix = im.load()
    width = im.size[0]
    height = im.size[1]
    for x in range(height):
        for y in range(width):
            value = round(255 * p[x][y][j])
            pix[x, y] = (value,value,value)
    return pix
```

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210728224400540.png" alt="image-20210728224400540" style="zoom:60%;" /><img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210728005057782.png" alt="image-20210728005057782" style="zoom: 60%;" /><img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210728004953116.png" alt="image-20210728004953116" style="zoom: 60%;" />

The classification result is good, the string 'non gene' can be clearly distinguished, but there is still some noise.

Last, the estimated mean vector and covariance matrix are

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210728005952088.png" alt="image-20210728005952088" style="zoom: 67%;" />

------------------------

**Initialization Ⅱ**

means_2:<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210728224931922.png" alt="image-20210728224931922" style="zoom:67%;" />

Visualization results of the GMM fit:

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210728224915620.png" alt="image-20210728224915620" style="zoom:40%;" />

We can also see that all three classifications in this initialization converge to valid values, and have a good classification result. Then create and display normalized images for each category based on posterior probabilities.

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210728225006314.png" alt="image-20210728225006314" style="zoom:60%;" /><img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210728225026522.png" alt="image-20210728225026522" style="zoom:60%;" /><img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210728225040063.png" alt="image-20210728225040063" style="zoom:60%;" />

The classification result is also good, but in the first class of normalized images, there is more noise.

Last, the estimated mean vector and covariance matrix are

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210728225100966.png" alt="image-20210728225100966" style="zoom:67%;" />

----------------

**Initialization Ⅲ**

means_3:<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729012224498.png" alt="image-20210729012224498" style="zoom:67%;" />

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729011936237.png" alt="image-20210729011936237" style="zoom: 40%;" />

Only two classifications converge to valid values.

Then create and display normalized images for each category based on posterior probabilities.

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729012006154.png" alt="image-20210729012006154" style="zoom:60%;" /><img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729012126048.png" alt="image-20210729012126048" style="zoom:60%;" /><img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729012152396.png" alt="image-20210729012152396" style="zoom:60%;" />

The classification result is bad, the strings cannot be distinguished, and even the normalized image of the third category becomes all black.

Last, the estimated mean vector and covariance matrix are

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729012238789.png" alt="image-20210729012238789" style="zoom:67%;" />

**Comment**:

By visualizing the GMM fitting results, we can conclude that different initializations have an effect on the GMM fitting.

A not "good" initialization may lead to non-convergence of the classification results and not getting K valid categories, e.g., the mean of the third category in initialization Ⅲ does not converge to a valid value, and the normalized image becomes an all-black plot. And, different initializations can lead to different classification results. Although the mean values of initialization Ⅰ and initialization Ⅱ both converge to the valid values and the differences in the GMM fit plots are small, it can be seen based on the normalized images that the results of initialization Ⅱ generate more noise than initialization Ⅰ. **So among these three random initializations, the result of initializing Ⅰ is a "best" result.**

The above initialization is generated randomly using uniform distribution. However, observing the scatter plot of the pixel points, it is found that the pixel points are in a very concentrated region, and using such an initialization method may lead to results that do not converge to valid values. So a new random initialization method is considered, where $K=3$ points are randomly selected as the initial mean of the GMM among the sample points in the $rg$ chromaticity space. This means that the purpose of randomly generated initialization is achieved and the possibility of non-convergence of the results is reduced. And through experiments, the obtained results converge to the valid values and the classification results are good.

------------------------

(c) Repeat the exercise in Step 1b for $K = 4$ and $K = 5$. Comment on the results.  

**Solution**:

#### 1. $\bf{K=4}$

**Initialization Ⅰ**

means_1:<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729015843785.png" alt="image-20210729015843785" style="zoom:67%;" />

Visualization results of the GMM fit:

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729015828238.png" alt="image-20210729015828238" style="zoom:40%;" />

Only three classifications converge to valid values.

Then create and display normalized images for each category based on posterior probabilities.

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729020425181.png" alt="image-20210729020425181" style="zoom:67%;" />

The string 'non gene' can be clearly distinguished, but one of the normalized images is all black.

Last, the estimated mean vector and covariance matrix are

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729020444426.png" alt="image-20210729020444426" style="zoom:67%;" />

------------------------

**Initialization Ⅱ**

means_2:<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729021054133.png" alt="image-20210729021054133" style="zoom:67%;" />

Visualization results of the GMM fit:

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729021109356.png" alt="image-20210729021109356" style="zoom:40%;" />

We can see that all four classifications in this initialization converge to valid values, and have a good classification result. Then create and display normalized images for each category based on posterior probabilities.

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729021549810.png" alt="image-20210729021549810" style="zoom:67%;" />

The classification result is good, the string 'non gene' can be distinguished, but there is still some noise.

Last, the estimated mean vector and covariance matrix are

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729021607431.png" alt="image-20210729021607431" style="zoom:67%;" />

----------------

**Initialization Ⅲ**

means_3:<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729022338774.png" alt="image-20210729022338774" style="zoom:67%;" />

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729022326036.png" alt="image-20210729022326036" style="zoom:40%;" />

We can also see that all four classifications in this initialization converge to valid values, and have a good classification result. Then create and display normalized images for each category based on posterior probabilities.

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729022518886.png" alt="image-20210729022518886" style="zoom:67%;" />

The classification result is also good, the string 'non gene' can be distinguished, but there is still some noise. And as far as the results of normalized images are concerned, the classification effect of initialization Ⅲ is not much different from that of initialization Ⅱ.

Last, the estimated mean vector and covariance matrix are

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729022355922.png" alt="image-20210729022355922" style="zoom:67%;" />



**Comment**:

The overall conclusion has been summarized in the case of $K = 3$. Only the results of different initializations for the $K=4$ case are commented here.

The mean value of one of the categories in initialization Ⅰ did not converge to a valid value. And initialization Ⅱ and initialization Ⅲ have similar performance on classification results and normalized images. **So among these three random initializations, the result of both initialization Ⅱ and initialization Ⅲ is a "best" result.**



------------

#### 2. $\bf{K=5}$

**Initialization Ⅰ**

means_1:<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729023225237.png" alt="image-20210729023225237" style="zoom:67%;" />

Visualization results of the GMM fit:

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729023237969.png" alt="image-20210729023237969" style="zoom:40%;" />

Only four classifications converge to valid values.

Then create and display normalized images for each category based on posterior probabilities.

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729023527633.png" alt="image-20210729023527633" style="zoom:67%;" />

The string 'non gene' can be distinguished, but there is lots of noise. And one of the normalized images is all black.

Last, the estimated mean vector and covariance matrix are

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729023541898.png" alt="image-20210729023541898" style="zoom:50%;" />

------------------------

**Initialization Ⅱ**

means_2:<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729024222354.png" alt="image-20210729024222354" style="zoom:67%;" />

Visualization results of the GMM fit:

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729024204579.png" alt="image-20210729024204579" style="zoom:40%;" />

We can see that all five classifications in this initialization converge to valid values. But this may not be a good classification result, because we can see that there are three classifications with mean centers very close to each other.

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729024527308.png" alt="image-20210729024527308" style="zoom:67%;" />

The classification result is not bad. The string 'non gene' can be distinguished, but there is lots of noise. 

Last, the estimated mean vector and covariance matrix are

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210729024235886.png" alt="image-20210729024235886" style="zoom:50%;" />

----------------

**Initialization Ⅲ**

means_3:<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210730020650074.png" alt="image-20210730020650074" style="zoom:67%;" />

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210730020704205.png" alt="image-20210730020704205" style="zoom:40%;" />

We can also see that all five classifications in this initialization converge to valid values, and have a good classification result. Then create and display normalized images for each category based on posterior probabilities.

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210730021022064.png" alt="image-20210730021022064" style="zoom:67%;" />

The classification result is good, the string 'non gene' can be clearly distinguished.

Last, the estimated mean vector and covariance matrix are

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210730020726267.png" alt="image-20210730020726267" style="zoom: 50%;" />

**Comment**:

The overall conclusion has been summarized in the case of $K = 3$. Only the results of different initializations for the $K=5$ case are commented here.

The mean value of one of the categories in initialization Ⅰ did not converge to a valid value. And although the classification results of initialization Ⅱ all converge to valid values, the result is not a good one.  Initialization III performs well on both classification results and normalized images, and the images can be clearly distinguished with little noise. **So among these three random initializations, the result of initialization Ⅲ is a "best" result.**

In summary, the classification results for $K=3$ and $K=5$ were better than $K=4$. Although the results of all three classifications can distinguish the string 'non gene', $K=5$ is the clearest. However, both $K=4$ and $K=5$ showed overfitting, i.e., the pink color within the string was also divided into several categories. So $K=3$ is optimal when overfitting is not desired, $K=5$ is optimal when overfitting is allowed.

