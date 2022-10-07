

# matlab

exp() e的指数

log()   ->ln

log10 log2

sin() cos() tan()

sum() mean()均值 std()标准差 var()方差 cov()协方差 max() min()

range()极差 median()中位数 mode()众数

1:10 

reshape(矩阵,[a,b])

linspace(start, end, 点数)

eye(n) or eye([a,b]) 单位阵

zeros() ones()

rand 均匀分布随机数

randi均匀分布伪随机整数

randn正态分布随机数

randperm整数随机排列

[a,b] = size() 行x列

A'转置  inv(A)逆矩阵

[V,D] = eig(A)特征值和特征向量 V:特征向量 D:特征值矩阵

直接运算符是正常矩阵运算

.运算符是对应元素运算

广播机制：不能运算的矩阵给你补成能运算的再运算

A == B 对应位置相等返回矩阵对应位置为1 反之为0

matlab按列索引

str2num("5") 字符串5转数值5

num2str(1/3, 2) 	第二个数字为位数

input("")

disp() display 输出语句

&|~与或非

if()

elseif()

else
end

isprime()

for i = value %类似python
	yuju
end

while
yuju
end
function [输出] = myfun(输入)

end;

function s = area(r)
	s = pi * r.^2
end

可以多返回

function [s,c] = areaAndLen(r)
	s = pi * r.^2
	c = 2 * pi * r
end

匿名函数

f = @(输入) 函数体

f = @(x)x.^2;
f(2) f([2, 3])

匿名和有名转换

f1 = @area

# # plot

```matlab
x = linspace(0,2*pi,1000);
y = sin(x);
plot(x, y)
%%%
x = linspace(0,2*pi,1000);
y1 = sin(x);
y2 = cos(x);
plot(x, y1)
hold on;
plot(x, y2)
%%%
plot(x,y1,x,y2)
%%%
x = linspace(0,2*pi,1000);
y1 = sin(x);
y2 = cos(x);
plot(x,y1,x,y2);
xlabel('x\m')
ylabel('y1=sinx, y2=cosx')
title('Function')
legend('sinx','cosx') %图例
%%%开多个图层
x = linspace(0,2*pi,1000);
y1 = sin(x);
y2 = cos(x);
figure;
plot(x,y1);
figure;
plot(x,y2);
%%% subplot(m,n,i) m行n列第i张图
x = linspace(0,2*pi,1000);
y1 = sin(x);
y2 = cos(x);
subplot(1,2,1);
plot(x,y1);
subplot(1,2,2);
plot(x,y2);
%%%
plot(x, y, '- r o') % 线型 颜色 描点类型
%线形 - -- : -.
%描点类型 . o x + * < > ^ v s方 d菱 p五星 h六星
%颜色 r g b y w k黑
%线宽 'lineWidth',1,......
%描点边框颜色 'MarkerEdgeColor', 'r',...
%描点内部填充颜色 "MarkerFaceColor",'y',...
%描点大小 'MarkerSize',10,......
%%%
grid on;
axis on;
axis([xmins, xmax, ymins, ymax])
axis squal
axis square
%%% 生成gif图
frame = getframe(gcf);
    I = frame2im(frame);
    [I,map ] = rgb2ind(I,256);
    if i == 500    		imwrite(I,map,'test3.gif','gif','LoopCount',inf,'DelayTime',0.2);
    else        imwrite(I,map,'test3.gif','gif','WriteMode','append','DelayTime',0.2);
    end
   %%%
   errorbar:含误差条线图
   errorbar(x,y,err)
   x = linspace(0,100,10);
	 y = x .^ 0.5;
	 err = rand(size(x));
	 errorbar(x,y,err,'both','LineWidth',2);
   
   histogram: 直方图
   histofram(x,n) 基于x创建直方图，n为区间数量
   x = randn([1,100000]);
	n = 50;
	histogram(x,n,'LineWidth',1.5)

scatter(x,y)散点图
x = linspace(0,2*pi,50);
y = sin(x);
scatter(x,y)

bar（y）柱状图
pie(x,explode)饼图

```

# #三维曲线

```matlab
三维曲线 plot3
x = linspace(1,6*pi,200);
y = sin(x);
z = cos(x);
plot3(y,z,x,"LineWidth",2)
xlabel('x')
ylabel('y = sinx')
zlabel('z = cosx')
title('y = sinx , z = cosx')


散点图 scatter3

三维曲面 mesh，surf
[X,Y] = meshgrid(x,y) % 列数取决于x，行数取决于y 创建空间
mesh(x,y,z);

```

# #文件操作

```matlab
将数据写入文件 writetable(txt、Excel)
m = rand(4) + 1;
round(m,2,"significant");
t = table(m)
writetable(t,'m.xls');
writetable(t,'m.txt','Delimiter','\t','WriteVariableNames',false) %如果要追加，换掉矩阵，用'WriteMode','append'追加到之后
type m.txt


从文件读取数据 readtable (filename)
t = readtable('m.txt')
m = table2array(t)

t = readtable('m.xls','Sheet',2,'Range','B2:E4') %从表单中获取


```

# #图片处理

```matlab
读取照片
imread('路径')
生成渐变
bw = zeros([256,400]);
for i = 1:256
    for j=1:400
        bw(i,j) = i-1;
    end
end
bw = uint8(bw);
imshow(bw);

rgb分离与合并
pepper = imread("peppers.png");
imshow(pepper);
R = pepper(:,:,1)
G = pepper(:,:,2)
B = pepper(:,:,3)
subplot(2,2,1);
imshow(R);
subplot(2,2,2);
imshow(G);
subplot(2,2,3);
imshow(B);

彩色图转灰度图 rgb2gray
pepper_gray = rgb2gray(pepper);
imshow(pepper_gray);

二值化imbinarize
pepper_gray = rgb2gray(pepper);
imshow(pepper_gray)
[row,col ] = size(pepper_gray);
for i = 1:row
    for j = 1:col
        if(pepper_gray(i,j) > 128)
            pepper_gray(i,j) = 1;
        else
            pepper_gray(i,j) = 0;
        end
    end
end
figure;
imshow(logical(pepper_gray))

pepper_gray = rgb2gray(pepper)
bw = imbinarize(pepper_gray,"adaptive","Sensitivity",0.2)
imshow(bw)

调整图像大小
imresize(pic, 倍数/[sizex, sizey]);

旋转图像
imrotate(pic,45);

图像加减乘除
imadd(pic, 50);

直方图
I = imread(pic);
imhist(I);

直方图均衡化histeq
J = histeq(I);

标注连通分量
L = bwlabel(BW, n) n为连同分量数。 

优化二值化
I = imread('coin.png');
imshow(I)
J = imsubtract(I,imopen(I, strel('disk',50))); %开运算，先腐蚀后膨胀
bw = imbinarize(J)
imshow(bw)

L = bwlabel(bw)
max(max(L)) %显示几个硬币
```

# #解方程

```matlab
多项式合并(解析解)
syms x
(x + 3 * x - 5 * x ) * x / 4  

解方程（解析解）
syms a b c x
solve(a*x.^2 + b .* x + c, x)

解方程组（解析解）
syms a b x y
y1 = x + b*y - 5;
y2 = a*x - y - x;
res = solve(y1,y2,x,y);
res.x
res.y

解方程（解析解）
f = @(x)2*x - x^2-exp(-x);
fsolve(f,0)

解方程组（解析解）
a = 3;
b = 5;
f = @(x)funs(x,a,b);
fsolve(f,[0,0])
function y = funs(x, a, b)
    y(1) = x(1) + b*x(2) -5;
    y(2) = a*x(1) - x(2) - x(1);
end

常微分方程（解析解）
syms y(x)
y1 = diff(y) == 2 * x;
dsolve(y1, y(0) == 1)

syms y(x) mu
eqn = diff(y,2) == mu * diff(y) + y;
cond1 = y(0) == 1;
Dy = diff(y);
Dy(0) == 0;
dsolve(eqn)

常微分方程组（解析解）
syms y1(x) y2(x)
eqn1 = diff(y1) == y2;
eqn2 = diff(y2) == -y1;
cond1 = y1(0) == 1;
cond2 = y2(0) == 1;
dsolve(eqn1,eqn2,cond1,cond2)

常微分方程（数值解）ode45 ode23 ode113 ode15s ode23s
f = @(x,y)2 * x;
tspan = [0,10];
y0 = 10;
[x,y] = ode45(f,tspan,y0);
plot(x,y)

常微分方程组（数值解）
mu = 1;
f = @(x,y)fun(y,mu);
tspan = [0,20];
y0 = [1 0];
[x,y] = ode45(f,tspan,y0);
plot(x,y);
function ydot = fun(y, mu)
    ydot = [y(2); mu*(1-y(1)^2)*y(2) + y(1)];
end

f = @(x,y)fun(y, a, b);
y0 = [3 4];
a = 100;
b = 50;
[x,y] = ode45(f,[0 10],y0);
plot(x,y);


function ydot = fun(y, a, b)
    ydot = [a-(b+1)*y(1) + y(1)^2 * y(2); b*y(1) - y(1)^2 * y(2)];
end






















```

http://43.142.53.63:8888/tencentcloud

telkwevr

e1e35cff43a5