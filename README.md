### Matlab图像锐化与像素坐标选取

~~~matlab
image0= imread('/Users/ihrem/Desktop/6Point.jpg');%读取文件
image0 = rgb2gray(image0);
image0 = im2double(image0);
[M,N] = size(image0);

%显示图片
% figure;
% subplot(2,2,1);
% imshow(image0);
% title('Y YuanTu')

%以下是用matlab分别计算函数各抽样点的傅里叶变换谱的幅角与模,并对各点的模归一化
object=fft2(image0);
object=fftshift(object);%用matlab中的移谱函数fftshift( )将频谱的低频成分移到中心,以避免再现时像分散在边缘
object=abs(object);
imgdata=1000*object/max(max(object));
% figure();
% imshow(imgdata);
fig=figure
P1= 1 * (imgdata .^ 1.2);
P2= 1 * (imgdata .^ 1.4);
P3= 1 * (imgdata .^ 1.6);
P4= 1 * (imgdata .^ 1.8);
P5= 1 * (imgdata .^ 100.0); % 减少周围的噪点
img=mapminmax(imgdata, 0, 1);
% figure
% imhist(img);
% title('灰度直方图');

% figure
% subplot(2,3,1);
% figure
% title('原始图像');
% figure
% imshow(P1)
% title('伽马变换:c=1,γ=1.2')
% figure
% imshow(P2)
% title('伽马变换:c=1,γ=1.4')
% figure
% imshow(P3)
% title('伽马变换:c=1,γ=1.6')
% figure
% imshow(P4)
% title('伽马变换:c=1,γ=1.8')

imshow(P5)
title('伽马变换:c=2,γ=2.0')

frame = getframe(fig); % 获取frame
img = frame2im(frame); % 将frame变换成imwrite函数可以识别的格式
imwrite(img,'a.png'); % 保存到工作目录下，名字为"a.png"


close all; clc;
%I = imread('lena.jpg');
I = imread('a.png');
[m,n,p]=size(I);
if p==3 
%     fprintf('彩色图\n');
%     fprintf('x\ty\tR\tG\tB\n');
else
    fprintf('灰度图\n');
    fprintf('x\ty\tY\n'); 
end
     
for i=221:414
    for j=158:328
        if p==3 
           fprintf('%d\t%d\t%d\t%d\t%d\n',i,j,I(i,j,1),I(i,j,2),I(i,j,3));
        end
    end
end
imshow(P5)
~~~



### 优化后

~~~matlab
image0= imread('/Users/ihrem/Desktop/6Point.jpg');%读取文件
image0 = rgb2gray(image0);
image0 = im2double(image0);
[M,N] = size(image0);

%显示图片
% figure;
% subplot(2,2,1);
% imshow(image0);
% title('Y YuanTu')

%以下是用matlab分别计算函数各抽样点的傅里叶变换谱的幅角与模,并对各点的模归一化
object=fft2(image0);
object=fftshift(object);%用matlab中的移谱函数fftshift( )将频谱的低频成分移到中心,以避免再现时像分散在边缘
object=abs(object);
imgdata=1000*object/max(max(object));
% figure();
% imshow(imgdata);
fig=figure
P1= 1 * (imgdata .^ 1.2);
P2= 1 * (imgdata .^ 1.4);
P3= 1 * (imgdata .^ 1.6);
P4= 1 * (imgdata .^ 1.8);
P5= 1 * (imgdata .^ 100.0); % 减少周围的噪点
img=mapminmax(imgdata, 0, 1);
% figure
% imhist(img);
% title('灰度直方图');

% figure
% subplot(2,3,1);
% figure
% title('原始图像');
% figure
% imshow(P1)
% title('伽马变换:c=1,γ=1.2')
% figure
% imshow(P2)
% title('伽马变换:c=1,γ=1.4')
% figure
% imshow(P3)
% title('伽马变换:c=1,γ=1.6')
% figure
% imshow(P4)
% title('伽马变换:c=1,γ=1.8')

imshow(P5)
title('伽马变换:c=2,γ=2.0')

frame = getframe(fig); % 获取frame
img2 = frame2im(frame); % 将frame变换成imwrite函数可以识别的格式
imwrite(img2,'a.png'); % 保存到工作目录下，名字为"a.png"


close all; clc;
%I = imread('lena.jpg');
I = imread('a.png');
[m,n,p]=size(I);
if p==3 
%     fprintf('彩色图\n');
%     fprintf('x\ty\tR\tG\tB\n');
else
    fprintf('灰度图\n');
    fprintf('x\ty\tY\n'); 
end
c1=[];
c2=[];
c3=[];
x=[];
y=[];
aa=0;
for i=221:1:414
    for j=158:1:351 
%         fprintf('%d\t%d\t%d\t%d\t%d\n',i,j,I(i,j,1),I(i,j,2),I(i,j,3));
              t1=I(i,j,1);
              c1=[c1,t1];
              t2=I(i,j,2);
              c2=[c2,t2];
              t3=I(i,j,3);
              c3=[c3,t3];
              aa=aa+1
              x=[x,aa];
              y=[y,aa];
    end
end
% x=0:1:194;
% y=0:1:194;
% c1=I(221:1:414,158:1:351,1)
% c2=I(221:1:414,158:1:351,2)
% c3=I(221:1:414,158:1:351,3)
xy=[x;y;c1;c2;c3];

id = fopen('xy.txt','w');
fprintf(id,'%d     %d     %d     %d     %d\n',xy)


~~~

## 优化

~~~matlab
image0= imread('/Users/ihrem/Desktop/6Point.jpg');%读取文件
image0 = rgb2gray(image0);
image0 = im2double(image0);
[M,N] = size(image0);
object=fft2(image0);
object=fftshift(object);%用matlab中的移谱函数fftshift( )将频谱的低频成分移到中心,以避免再现时像分散在边缘
object=abs(object);
imgdata=1000*object/max(max(object));
fig=figure
P5= 1 * (imgdata .^ 100.0); % 减少周围的噪点
img=mapminmax(imgdata, 0, 1);
imshow(P5)
title('伽马变换:c=2,γ=2.0')
frame = getframe(fig); % 获取frame
img2 = frame2im(frame); % 将frame变换成imwrite函数可以识别的格式
imwrite(img2,'a.jpg'); % 保存到工作目录下，名字为"a.png"
close all; clc;
I = imread('a.jpg');
% [m,n,p]=size(I);
cropimg_2 = imcrop(I, [278 178 200 200]); %(dat,[XMIN YMIN WIGTH HEIGHT])
fig2=figure
imshow(cropimg_2)
frame2 = getframe(fig2); % 获取frame
img3 = frame2im(frame2); % 将frame变换成imwrite函数可以识别的格式
imwrite(img3,'b.jpg'); % 保存到工作目录下，名字为"b.png"

I2 = imread('b.jpg');

[m,n,p]=size(cropimg_2);
if p==3 
%     fprintf('彩色图\n');
%     fprintf('x\ty\tR\tG\tB\n');
else
    fprintf('灰度图\n');
    fprintf('x\ty\tY\n'); 
end
c1=[];
c2=[];
c3=[];
x=[];
y=[];
for i=1:m
    for j=1:n 
%         fprintf('%d\t%d\t%d\t%d\t%d\t%d\n',i,j,cropimg_2(i,j,1),cropimg_2(i,j,2),cropimg_2(i,j,3));
              t1=I2(i,j,1);
              c1=[c1,t1];
              t2=I2(i,j,2);
              c2=[c2,t2];
              t3=I2(i,j,3);
              c3=[c3,t3];
              x=[x,i];
              y=[y,j];
    end
end
xy=[x;y;c1;c2;c3];

id = fopen('xymmin.txt','w');
fprintf(id,'%d     %d     %d     %d     %d\n',xy)
~~~

