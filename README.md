# Guided filter for OpenCV

Guided filter is an edge-preserving smoothing filter like the bilateral filter. It is straightforward to implement and has linear complexity independent of the kernel size. For more details about this filter see [[Kaiming10]](http://research.microsoft.com/en-us/um/people/kahe/eccv10/).

The interface consists of one simple function `guidedFilter` and a class `GuidedFilter`. If you have multiple images to filter with the same guidance image then use `GuidedFilter` class to avoid extra computations on initialization stage. The code supports single-channel and 3-channel (color) guidance images and `CV_8U`, `CV_8S`, `CV_16U`, `CV_16S`, `CV_32S`, `CV_32F` and `CV_64F` data types.


## Examples

These examples are adapted from the [original MATLAB implementation](http://research.microsoft.com/en-us/um/people/kahe/eccv10/guided-filter-code-v1.rar).

### Smoothing

```c++
cv::Mat I = cv::imread("./img_smoothing/cat.bmp", CV_LOAD_IMAGE_GRAYSCALE);
cv::Mat p = I;

int r = 4; // try r=2, 4, or 8
double eps = 0.2 * 0.2; // try eps=0.1^2, 0.2^2, 0.4^2

eps *= 255 * 255;

cv::Mat q = guidedFilter(I, p, r, eps);
```

### Flash/no-flash denoising

```c++
cv::Mat I = cv::imread("./img_flash/cave-flash.bmp", CV_LOAD_IMAGE_COLOR);
cv::Mat p = cv::imread("./img_flash/cave-noflash.bmp", CV_LOAD_IMAGE_COLOR);

int r = 8;
double eps = 0.02 * 0.02;

eps *= 255 * 255;

cv::Mat q = guidedFilter(I, p, r, eps);
```

![Cave Flash](http://atilimcetin.com/guided-filter/img_flash/cave-flash.png)
![Cave No Flash](http://atilimcetin.com/guided-filter/img_flash/cave-noflash.png)
![Cave Denoised](http://atilimcetin.com/guided-filter/img_flash/cave-denoised.png)


### Feathering

```c++
cv::Mat I = cv::imread("./img_feathering/toy.bmp", CV_LOAD_IMAGE_COLOR);
cv::Mat p = cv::imread("./img_feathering/toy-mask.bmp", CV_LOAD_IMAGE_GRAYSCALE);

int r = 60;
double eps = 1e-6;

eps *= 255 * 255;

cv::Mat q = guidedFilter(I, p, r, eps);
```

### Enhancement

```c++
cv::Mat I = cv::imread("./img_enhancement/tulips.bmp", CV_LOAD_IMAGE_COLOR);
I.convertTo(I, CV_32F, 1.0 / 255.0);

cv::Mat p = I;

int r = 16;
double eps = 0.1 * 0.1;

cv::Mat q = guidedFilter(I, p, r, eps);

cv::Mat I_enhanced = (I - q) * 5 + q;
```



## License

MIT license.

