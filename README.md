基于神经网络的人脸识别Rest API

### 功能介绍：

方便各类不懂caffe和图像处理的人群使用，将人脸识别中常用的功能封装成了Rest Api，部署后就可以直接调用。目前支持人脸相识度对比，人脸定位，年龄检测，性别检测。在未来，如果有空将持续更新，下一步将继续添加表情，心情状态检测等功能

### 需要环境：
opencv

caffe

django

.....

还有其他一堆库，懒得写了，你运行的时候缺哪个就装哪个吧，反正都是可以通过pip安装的

### 识别结果：
<img src="result/r1.png"> <br>
<img src="result/r2.png"> <br>
<img src="result/r3.png"> <br>

### 识别精度说明：

人脸对比功能还达不到用做Face ID的级别，如果用于一般精度要求不是很高的项目，判断是否是同一个人问题不大。性别检测功能基本完美，能达到人类的水平。年龄的话在大天朝美图神技面前无解，如果是没经过美图的照片基本上准确，如果经过美图.......人脸定位的话，偶尔会把非人脸判断进去


### 文件说明：

model 文件夹存放的是一些模型文件
RestServer是核心代码

项目将人脸识别与人脸定位的功能封装成了REST API，将RestServer运行起来以后就可以通过REST API的方式访问调用

### 运行方法：

#### 环境搭建：

Ubuntu:

使用ubuntu17+的系统能保证你最快时间搭建出一套运行环境，我写这个README的时候UBUNTU18.04还没出来，所以不保证18+的兼容性。

为了你最快搭建出这个项目的试用环境，所以我下面的项目都是最简安装，没考虑性能

ubuntu 17.10 +python3 环境搭建：

```Bash
sudo apt-get install python3 pip3
sudo pip3 install django
sudo pip3 install sklearn
sudo apt-get install libopencv-dev python3-opencv
sudo apt-get install caffe-cpu
```

* 如需要使用GPU（运行更快，但并非所有电脑都能支持）请安装GPU版Caffe，CUDA须用apt安装
```Bash
sudo apt-get install python3 pip3
sudo pip3 install django
sudo pip3 install sklearn
sudo apt-get install libopencv-dev python3-opencv
sudo apt-get install nvidia-cuda-toolkit
sudo apt-get install caffe-cuda
```





进入RestServer/config文件夹，打开config.py文件，修改成你本机的路径，然后在RestServer目录下运行python3 manage.py runserver 没有报错即可

然后打开项目目录下的test.html（任意浏览器打开）即可测试项目功能


Windows:

前提是你已经装好python3和pip

```Bash
sudo pip3 install django
sudo pip3 install sklearn
```

去[caffe windows](https://github.com/BVLC/caffe/tree/windows)下载编译好的（你自己编译当然没问题）Windows caffe二进制文件，
注意选择你机器对应的版本（如果你不知道怎么选，那就选 CPU only, Python 3.5: Caffe Release），下载下来后直接解压即可


进入RestServer/config文件夹，打开config.py文件，修改成你本机的路径，然后在RestServer目录下运行python3 manage.py runserver 没有报错即可

然后打开项目目录下的test.html（任意浏览器打开）即可测试项目功能


Other：

如果是线上使用或者你无法使用上面的环境，那么你需要自己搭建运行环境，项目主要需要的模块有3个caffe,opencv,django,可以参照caffe官网安装caffe，其他2个用pip安装就行了


### API调用方法：

<table>
	<tr><td>接口地址</td> <td>方法</td> <td>参数</td> <td>说明</td>  <td>返回参数</td>  </tr>
	<tr>
	<td>http://你的ip/compared </td> <td> POST</td> <td>face1 和 face2 数据为你的两张人脸图片 </td>  <td>人脸对比接口地址</td>
	 <td > JSON数组，status调用是否成功，data为两张人脸的相似度（大约78%可判断为同一个人），msg为说明，runtime为识别执行时间
	</td>
	</tr>
	<tr>
	<td>http://你的ip/locate  </td> <td> POST</td> <td>pic 数据为你需要定位人脸的图片 </td>  <td>人脸定位接口地址</td>
	 <td > JSON数组，返回中每有一个数组就表示检测到一张人脸 X，Y表示人脸左上角坐标，height width表示高度和宽度
	</td>
	</tr>
	<tr>
		<td>http://你的ip/gender</td><td>POST</td><td>pic 数据为你需要检测的人脸图片</td><td>性别检测接口</td>
	<td > JSON数组，status调用是否成功，data为Female 或者male，msg为说明，runtime为识别执行时间
	</td>
	</tr>
	<tr>
		<td>http://你的ip/age</td><td>POST</td><td>pic 数据为你需要检测的人脸图片</td><td>年龄检测接口</td>
			<td > JSON数组，status调用是否成功，data 为一个年龄区间 ，runtime为识别执行时间
	</td>
	</tr>
	<tr>
		<td>http://你的ip/expression</td><td colspan="4">表情检测，功能待实现。。。</td>
	</tr>
</table>


### 部署说明：

如果你需要用于生产项目使用，该项目一定要用apache2或者ngnix发布，上面提供的运行方法仅供测试使用。具体部署访问请参考[django项目部署](https://docs.djangoproject.com/en/2.0/howto/deployment/)



### 其他说明：

关于算法，人脸定位算法来自[openCv2](https://opencv.org/);性别和年龄检测来自 [Age and Gender Classification using Convolutional Neural Networks](https://www.openu.ac.il/home/hassner/projects/cnn_agegender/);人脸相识度对比来自[VGG FACE](http://www.robots.ox.ac.uk/~vgg/software/vgg_face/)


### 最后！！

欢迎提出意见，如果改进优化了这个项目请也提交我一份代码，邮箱地址 ok@xjiangwei.cn
