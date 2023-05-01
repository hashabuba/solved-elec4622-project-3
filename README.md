Download Link: https://assignmentchef.com/product/solved-elec4622-project-3
<br>
This is the second project, to be demonstrated and assessed within the regular scheduled laboratory session in Week 13. The project is worth a nominal 10 marks, but optional bonus marks are available.

In this project, you explore various aspects of block-based motion estimation and global motion estimation, including sub-pixel precision motion compensation and estimation of the reliability of block motion vectors. You should review the lecture notes on motion estimation and also download and study the materials entitled “Lab 5” on the class web-site, since these provide you with an introduction to block-based motion estimation.

The last hour of your scheduled lab/tut session in Week 12 has been set aside to allow you to get help from the lab demonstrators in understanding this project’s objectives, and also understanding “Lab 5,” but you should be sure to take a look at these ahead of time to make the most out of this opportunity.

<h1>1           Global Motion Estimation</h1>

In this project, you will be estimating a global translational motion vector <strong>v </strong>from individual block motion vectors <strong>v</strong><em><sub>b</sub></em>, the subscript <em>b </em>here identifies individual blocks and runs from 1 to <em>B</em>, where <em>B </em>is the total number of blocks in the image. The estimation process involves to images (video frames) denoted here as <em>x</em>[<strong>n</strong>] and <em>y</em>[<strong>n</strong>], where <em>x</em>[<strong>n</strong>] is a source image and <em>y</em>[<strong>n</strong>] is the target image that will ultimately be approximated by motion compensation based on <em>x</em>[<strong>n</strong>]. The target frame <em>y</em>[<strong>n</strong>] is partitioned into <em>B </em>blocks, denoted <em>y<sub>b </sub></em>[<strong>n</strong>], and a motion vector <strong>v</strong><em><sub>b </sub></em>is estimated from

<strong>v</strong><em><sub>b </sub></em>= argmin<em>J<sub>b </sub></em>(<strong>u</strong>) <strong>u</strong>

where

where <em>p </em>= 1 for the SAD metric and <em>p </em>= 2 for the MSE metric.

Based on the estimated block motion vectors <strong>v</strong><em><sub>b</sub></em>, a global motion vector <strong>v </strong>can be found using a weighted least squares fitting procedure, as follows:

<strong>v </strong>= argmin<em>J</em><sub>fit </sub>(<strong>u</strong>) <strong>u</strong>

where

Here, the subscripts <em>x </em>and <em>y </em>denote the horizontal and vertical components of a motion vector. The solution to this minimization problem is easily found (e.g., by setting derivatives with respect to <em>u<sub>x </sub></em>and <em>u<sub>y </sub></em>to 0) to be

<strong>v</strong>

That is, one only needs to take the weighted average of the block motion vectors.

An important question is how the weights should be selected. To that end, in the last task of this project you will use a reliability metric inspired by the Harris corner detector’s figure of merit. Specifically, you will set

where Γ¯<em><sub>σ</sub></em><em>,b </em>is a 2 × 2 matrix formed from aggregating local matrices Γ<em><sub>σ </sub></em>[<strong>n</strong>] over the domain of block <em>b</em>, according to

Γ¯<em><sub>σ,b </sub></em>= [Γ<em><sub>σ </sub></em>[<strong>n</strong>]

<strong>n</strong>∈<em>b</em>

and

Here, ∇<em><sub>σ </sub></em>[<strong>n</strong>] is a scale dependent gradient vector that could be obtained by applying derivative of Gaussian

(DOG) filters to the target image <em>y</em>[<strong>n</strong>]. However, for simplicity, in this project you will use a difference of Gaussian approach, setting

where <em>y<sub>σ </sub></em>[<strong>n</strong>] is obtained by subjecting <em>y</em>[<strong>n</strong>] to a Gaussian low-pass filter with scale-parameter (standard deviation) <em>σ</em>.

You should make sure you understand why the weighting procedure described here should help in estimating more reliable global motion.

Global motion is only applicable to certain types of video content. In general, one should use a richer global motion model than pure translation — common model are the affine or perspective model, as described in your lecture notes. To simplify things for this project, we limit our attention to global translational motion, as described above, and test content suitable for exploring the solution will be uploaded to the class web-site for you to use in this project.

<h1>2           Tasks</h1>

Task 1: (4 marks) In your own time, complete the exercises in “Lab 5” — see the class web-site. In particular, you must complete the modifications to the “motion_example” workspace, which are required to produce a colour output image which simultaneously shows the target frame and the motion vectors. You should also be prepared to comment on (and demonstrate) the impact of different block sizes and search ranges, on motion compensated MSE, the motion vector field, and the visual quality of the motion compensated output image.

Task 2: (2 marks) Modify the code to fit a global translational motion vector <strong>v </strong>to the block motion vectors, as explained in Section 2, taking the weights <em>w<sub>b </sub></em>to be equal for all blocks <em>b</em>. The global vector <strong>v </strong>should have real-valued coordinates, not integer values, and your program should print the vector <strong>v</strong>.

Task 3: (3 marks) Further modify the code to perform motion compensation using the global motion vector <strong>v</strong>, rather than the individual estimated block motion vectors, reporting MSE and writing the motion compensated result out as an image. To do this, you will need to interpolate the original image samples, since <strong>v </strong>is not generally integer-valued, and for this purpose you should simply use bilinear interpolation. You can, if you like, continue to use the approach in “Lab 5,” where individual blocks are separately motion compensated, except that you must use the global motion vector in every block, and use bi-linear interpolation. Equivalently, you can simply shift the original image by <strong>v</strong>.

<ul>

 <li>Boundary extension is very important here. The code from “Lab 5” does not select motion vectors that will map a block beyond the boundaries of the source frame, but since a single global vector is being selected for all blocks, this constraint would force <strong>v </strong>to be <strong>0</strong>, which is not interesting. It follows that your modified code will need to extend the original source image by the maximum value of any block motion vector.</li>

 <li>Use the point-symmetric boundary extension method covered in Problem Set 1, problem 9, rather than the symmetric extension method.</li>

 <li>In addition to reporting MSE associated with the global motion vector <strong>v</strong>, your program should report the MSE associated with the originally estimated block motion vectors, so it will print out two MSE values, report the global motion vector <strong>v </strong>and write out the global motion compensated image for you to look at.</li>

 <li>Run the program on the two test sets provided on the web-site for evaluating global motion estimation.</li>

 <li>You will need to explore the use of different block sizes and be able to explain what happens as block size is changed.</li>

</ul>

Task 4: (1 mark) Modify the search criterion used in Task 3 to find motion vectors, so that the best vector is considered to be that which minimizes MSE over the block, rather than SAD. What impact does this have upon the two MSE values computed in Task 3 and the motion vector field? Can you find good explanations for your observations?

Task 5: (up to 4 bonus marks) As it stands, the global motion estimation strategy described above has a major weakness: it treats all block motion vectors as equally reliable. To address this, modify your program from Task 3 to compute a reliability weight <em>w<sub>b </sub></em>for each block <em>b </em>following the method proposed in Section 2, which is based on the figure of merit that is used in the Harris corner detector. This method requires you to perform Gaussian filtering, with scale parameter <em>σ</em>, take local horizontal and vertical differences, compute the 2 × 2 matrices Γ<em><sub>σ </sub></em>[<strong>n</strong>] and aggregate these over each block to form Γ¯<em><sub>σ</sub></em><em>,b</em>, after which the ratio of determinant to rank can be computed and used as the weight <em>w<sub>b</sub></em>. Your program should then find the weighted average of the block motion vectors <strong>v</strong><em><sub>b</sub></em>, using this as the global vector <strong>v</strong>.

<ul>

 <li>If your implementation does not involve any Gaussian filtering, the maximum number of bonus marks you can receive is limited to 2.</li>

 <li>If your program does not allow <em>σ </em>to be supplied on the command-line (at least covering the range 1 ≤<em>σ</em>≤ 4), the maximum number of bonus marks you can receive is limited to 3. Explore the improvements obtained in global motion compensated MSE when using this weighted scheme, in comparison to the unweighted approach of Task 3, for various block sizes and various values of <em>σ</em>. This is easiest to do, if your program accepts a command-line argument that can be used to disable the weighting process. This might be done, if you like, by specifying a meaningless value for <em>σ</em>, such as 0 or a negative value.</li>

</ul>