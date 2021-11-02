# AR_Tag_Detection
This repository has computer vision project. We detect AR tag and superimpose Lean's image on the AR Tag 
## AR-Tag Detection Pipeline
1) Finding AR-Tag.
2) Finding homographic matrix and using it to warp the AR-Tag.
3) Converting AR-tag into matrix.
4) Finding the ID and Orientation. 
5) Superimposing Lean's image on AR-Tag. 
## Finding AR-Tag
1) Image processing is done on the input frame of the video to remove noise using Gaussian Blur filter.
2) The filtered image from step 1 is coverted to gray scale and thresholded to a binary image. 
3) The Thresholded image is processed by findcontours() to find all the countours.
4) To seperate AR-Tag from countours detected we use two properties unique to AR-tag: (i) Area of AR-Tag and (11) coners of AR-Tag has black borders around it.
## Finding Homographic Matrix
1) Using the detected corner and world co-ordinates form the following matrix and let it be called "A"\
A=[x1, y1, 1, 0, 0, 0, -x2*x1, -x2*y1, -x2\
   0, 0, 0, x1, y1, 1, -y2*x1, -y2*y1, -y2\
   x3, y3, 1, 0, 0, 0, -x4*x3, -x4*y3, -x4\
   0, 0, 0, x3, y3, 1, -y4*x3, -y4*y3, -y4\
   x5, y5, 1, 0, 0, 0, -x6*x5, -x6*y5, -x6\
   0, 0, 0, x5, y5, 1, -y6*x5, -y6*y5, -y6\
   x7, y7, 1, 0, 0, 0, -x8*x7, -x8*y7, -x8\
   0, 0, 0, x7, y7, 1, -y8*x7, -y8*y7, -y8\]
2) Find the SVD decomposition of "A.
3) Extract the eigen-vector corresponding to the least eigen-value.
4) Reshape the result from step 3 to a 3*3 matrix  

## Coverting AR-Tag to AR-matrix
1) According to the height and width of the AR-tag image, the image is 
divided in 8x8 segments. 
2) Count the number of black and white pixels in each cell.
3) Based on the count of pixel if the number of black pixel is more than the white pixel we assign the cell as 0 else 1.

## Finding Tag-ID and Tag-Orientation

**Orientation**

- Case1:0 degree rotation\
If the value for matrix[2][2], matrix[2][5], matrix[5][2] is 0  while matrix[5][5] is 1. Then the angle is 0 degree.
- Case 2: 180 degree rotation\
If the value for matrix[2][2] is 1, while matrix[2][5], matrix[5][2] and matrix[5][5] is 0. Then the angle is 180 degree.
- Case 3: 90 degree rotation\
If the value for matrix[2][5] is 1, while matrix[2][2], matrix[5][2] and matrix[5][5] is 0. Then the angle is 90 degree.
- Case 4: -90 degree rotation\
If the value for matrix[5][2] is 1, while matrix[2][2], matrix[2][5] and matrix[5][5] is 0. Then the angle is -90 degree.

**Tag ID**
- Case1:0 degree rotation\
matrix[3][3] +matrix[4][3]*8 +matrix[4][4]*4 + matrix[3][4]*2
- Case 2: 90 degree rotation\
matrix[3][3]*2 + matrix[3][4]*4 + matrix[4][4]*8 + matrix[4][3]
- Case 3: 180 degree rotation\
matrix[3][3]*4 + matrix[4][3]*2 + matrix[4][4] + matrix[3][4]*8
- Case 4: -90 degree rotation\
matrix[3][3]*1 + matrix[3][4] + matrix[4][4]*2 + matrix[4][3]*4
 

