# EigenfaceClassification
In March 1991, researchers Turk &amp; Pentland from MIT released a paper titled "Face Recognition Using Eigenfaces" &lt;see here https://sites.cs.ucsb.edu/~mturk/Papers/mturk-CVPR91.pdf>. This project seeks to implement the methods proposed by these researchers using modern languages (Python) with the hope of achieving real-time face classification. 

My attempts have so far not been great :(, this is a work in progress.
Here is what has happened so far:

## 1. Eigenfaces generated
Part of the idea of this eigenfaces process, is that from a given dataset of images, using linear algebra and PCA (principle component analysis), just like the brain has a limited amount of connections but yet is still able to rapidly classify faces, representations of the dataset can be reduced to a lower dimensionality—and then reverted and the original image reconstructed with some 95% accuracy. 
Here is my flawed understanding of the process (sorry no LaTeX yet):
1. Take a dataset of images, of quantity M. Ideally they are images that have been corrected (i.e., actually faces and not a truck for example).
2. Generate the mean face. I have done this by loading the image np.asarray and flattened the matrix to be a (N^2, 1 really long!). Then performed a horizontal sum, and division by the images M to get the mean face which the paper calls Ψ (psi). Ψ contains the most significant features that are consistent across most images, or we can say the average features.
3. We now normalize the data-set by subtracting the mean face Ψ from all the M faces. We just want all the unique features! This new normalized data set we call A.
4. Now we want to generate a covariance matrix. The way to do this generally is is C = AA^T, but because it is a (N^2, M) x (M, N^2) calculation, the dimensionality would be (N^2, N^2) which would be too much to calculate! So instead Turk and Pentland suggest reducing the dimensionality to M x M by doing L = A^TA. What this means though is that we are now working in a L lower dimensionality.
5. Now we want to calculate the top k-eigenvectors. Some papers have a process to determine what is k, some don't. I won't for now. When we do our calculations, or better yet when **numpy import linalg** (i.e., e_value, e_vect = np.linalg.eig(L)) does it for us, we'll be able to calculate for eigenvalues and eigenvectors. The top k-eigenvectors are calculated by their pairing top k-eigenvalues.
6. These top k-eigenvectors are our lower dimensionality eigenfaces, not yet the eigenfaces. To get the eigenfaces you'll have to project them back onto the mean centered matrix A (np.dot(top k-eigenvectors, A). After doing so, you now have your true eigenfaces. The paper, and related papers, suggest that you can get a linear weighted combination of these eigenfaces (with the mean face) and reconstruct a known face ([and some have demonstrated unknown faces too](https://youtu.be/dN4hIUhjUt0?t=466)).
7. You can get the weights needed to reconstruct an image by first flattening it (1 x N^2), normalizing it (i.e., image - average face Ψ), and then projecting it onto the eigenfaces (another dot product). The resulting matrix is a (k, 1) matrix of weights.
8. Then to get the regenerated image it is the mean face Ψ + sum(w1 * e1, w2 * e2, ... wk * ek), where w are the weights and e are the eigenfaces.
9. Classification can be done by calculating euclidian distances between the images.

So far my data looks a bit odd. Looks like Mark Zucc turned into a different person :(:
| Data set | Eigenfaces | Process | Start, mean and final face |
-----------|------------|---------|----------------------------|
|![](https://api.llay.au/eigenfaces/dataset.png)| ![](https://api.llay.au/eigenfaces/eigenfaces.png) |  ![](https://api.llay.au/eigenfaces/weights_applied.png) | ![](https://api.llay.au/eigenfaces/reconstruction.png) |

# But Why?
It could be due to bad data used. My math needs to be reviewed again also.
