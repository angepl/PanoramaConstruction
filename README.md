# PanoramaConstruction

## Description
This is a python project using OpenCV to create a panorama that consists of five different but overlapping images. The goal is to apply a perspective transformation in each image, so that when you stitch them all together a panoramic view of their content is created.

## Methodology
- First of all we read the images with `cv2.imread()` function. This function loads the images in BGR format, so we use `cv2.cvtColor()` to change it to RGB. We load the images in left-to-right order and so we have `imgs[0]`, `imgs[1]`, `imgs[2]`, `imgs[3]` and `imgs[4]`.
- We create a SIFT object to calculate the images' keypoints and descriptors and a Brute-Force Matcher object to find the matches between pairs of images, whenever it is necessary.
- We create our panorama's frame by declaring its dimensions (`outer_x, outer_y`) and usisng `np.zeros()` function.
- We are going to use the middle image (`imgs[2]`) as a reference image, which means that we are going to put it in the middle of the panorama frame without any transformation, and transform all the other images according to this one.
- After putting `imgs[2]` in the middle of the frame we are going to save this image, call it `img3` and transform `imgs[1]` and `imgs[3]` in order to stitch them to `img3`. We calculate the keypoints and the descriptors for both pairs of images, we find their matches and we keep the good ones. `good2_3` holds the good matches between `imgs[1]` and `img3` while `good4_3` holds the good matches between `imgs[3]` and `img3`.
- We use these good matches to find the homography matrices that transform `imgs[1]` and `imgs[3]` into the wanted ones. This is accomplished by using the `cv2.findhomography()` function.
- Transform `imgs[1]` and `imgs[3]` using the homography matrices and `cv2.warpPerspective()` function.
- Put these two transformations in the same frame (in the right position).
- Put `imgs[2]` in the middle of this frame. This is going to be the first part of the panorama and I am going to store it in a variable called `panorama`.
- Repeat all the above using this first part (`panorama`) as a reference image, and trying to transform `imgs[0]` and `imgs[4]` to stitch them to it.
