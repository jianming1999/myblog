# import
import cv2
# read image file
img = cv2.imread("/Users/jianming241/jm/logo.png")
# create Image Window
cv2.namedWindow("Image")
# show Image
cv2.imshow("Image", img)
# wait
cv2.waitKey(0)
# destroy all windows
cv2.destroyAllWindows()


# write image file
cv2.imwrite("D:\\cat2.jpg", img)

### Usage

第一个参数是保存的路径及文件名，第二个是图像矩阵。其中，imwrite()有个可选的第三个参数，如下：

cv2.imwrite("D:\\cat2.jpg", img，[int(cv2.IMWRITE_JPEG_QUALITY), 5])

第三个参数针对特定的格式： 对于JPEG，其表示的是图像的质量，用0-100的整数表示，默认为95。 注意，cv2.IMWRITE_JPEG_QUALITY类型为Long，必须转换成int。
对于PNG，第三个参数表示的是压缩级别。cv2.IMWRITE_PNG_COMPRESSION，从0到9,压缩级别越高，图像尺寸越小。默认级别为3：

cv2.imwrite("./cat.png", img, [int(cv2.IMWRITE_PNG_COMPRESSION), 0])   

cv2.imwrite("./cat2.png", img, [int(cv2.IMWRITE_PNG_COMPRESSION), 9])  

