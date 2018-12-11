---
title: '@Octave tutorial'
comments: true
categories: languages
abbrlink: 65516c6f
date: 2017-09-04 09:50:29
updated: 2018-01-18 03:16:07
tags: octave
keywords:
description:
---


> Octave 是 Standard ML 公开课建议的初学者学习ML 的编程语言，其语法特性和 Matlab 几乎一样，但前者是开源的，免费的。本文内容转自该课的 Lecture Notes.

<!--more-->

## Basic Operations

```matlab
%% Change Octave prompt
PS1('>> ');
%% Change working directory in mac example:
cd Users/your username/desktop

%% elementary operations
5+6
3-2
5*8
1/2
2^6
1 == 2 % false
1 ~= 2 % true.  note, not "!="
1 && 0
1 || 0
xor(1,0)

%% variable assignment
a = 3; % 句尾加分号则不会立即将结果输出到 terminal 上，而是保存在内存中
b = 'hi';
c = 3>=1;

%% Displaying them:
a = pi
disp(a)
disp(sprintf('2 decimals: %0.2f',a))
disp(sprintf('6 decimals: %0.6f',a))
format long
a
format short
a

%% vectors and matrices
A = [1 2; 3 4; 5 6]

v = [1 2 3]
v = [1; 2; 3]
v = 1:0.1:2 % from 1 to 2, with stepsize of 0.1. Useful for plot axes.
v = 1:6 %from 1 to 6, with stepsize of 1(row vector)
C = 2*ones(2,3) % same as C = [2 2 2; 2 2 2]
w = ones(1,3) % 1×3 vector of ones
w = zeros(1,3)
w = rand(1,3) % draw from a uniform distribution
w = randn(1,3) % drawn from a normal distribution
w = -6 + sqrt(10)*(randn(1,10000));
hist(w) % plot histogram using 10bins (default)
hist(w, 50)
% note: if hist() crashes, try "graphics_toolkit('gnu_plot')" 

I = eye(4) % 4×4 identity matrix

% help function 
help eye
help rand
help help
```

## Moving Data Around

Data files used in this section: [featuresX.dat](https://raw.githubusercontent.com/tansaku/py-coursera/master/featuresX.dat), [priceY.dat](https://raw.githubusercontent.com/tansaku/py-coursera/master/priceY.dat)

```matlab
%% dimensions
sz = size(A) % 1x2 matrix: [(number of rows) (number of columns)]
size(A,1) % number of rows
size(A,2) % number of columns
length(v) % size of longest dimension

%% loading data
load('q1x.dat')
load q1y.dat
who % list variables in workspace
whos % list variables in workspace(detailed view)
v = q1x(1:10) % first 10 elements of q1x(counts down the columns)
save hello.mat v % save variable v into file hello.mat
save hello.txt v -ascii % save as ascii
% fopen, fread, fprintf, fscanf also work

%% indexing 
A(3,2) % indexing is (row,column)
A(2,:) % get the 2nd row
A(:,2) % get the 2nd column
A([1,3],:) % print all the elements of rows 1 and 3.
A(:,2) = [10; 11; 12] % change second row
A = [A, [100; 101; 102]]; %append column vec 分块矩阵
A(:) %select all elements as a column vector

% Putting data togenther
A = [1 2; 3 4; 5 6]
B = [11 12; 13 14; 15 16]
C = [A B] % concatenating A and B matrices side by side
C = [A, B] % concatenating A and B matrices side by side
C = [A; B] % concatenating A and B top and bottom
```

## Computing on Data

```matlab
%% initialize variabels
A = [1 2; 3 4; 5 6]
B = [11 12; 13 14; 15 16]
C = [1 1; 2 2]
v = [1;2;3]

%% matrix operations 
A * C % matrix multiplication
A .* B % element-wise multiplication
A .^ 2 % element-wise square of each element in A
1./v   % element-wise reciprocal
log(v)  % functions like this operate element-wise on vecs or matrices

exp(v)
abs(v)

-v % -1*v

v + ones(length(v),1)
% 上式等价于 v+1

A' % matrix transpose

%% misc useful functions

% max (or min)
a = [1 15 2 0.5]
val = max(a)
val = max(A) % if A a matrix, returns max from from each column

% compare values in a matrix & find
a < 3 % checks which values in a are less than 3
find(a < 3) % gives location of elements less than 3
A = magic(3) % generates a magic matrix - not much used in ML algorithms
[r,c] = find(A>=7)  % row, column indices for values matching comparison

% sum, prod
sum(a)
prod(a)
floor(a) % or ceil(a)
max(rand(3),rand(3))
max(A,[],1) -  maximum along columns(defaults to columns - max(A,[]))
max(A,[],2) - maximum along rows
A = magic(9)
sum(A,1)
sum(A,2)
sum(sum( A .* eye(9) ))
sum(sum( A .* flipud(eye(9)) ))


% Matrix inverse (pseudo-inverse)
pinv(A)        % inv(A'*A)*A'

```

## Plotting Data

```matlab
%% plotting 
t = [0:0.01:0.98];
y1 = sin(2*pi*4*t);
plot(t, y1);
y2 = cos(2*pi*4*t);
hold on;  % "hold off" to turn off
plot(t,y2,'r');
xlabel('time');
ylabel('value');
legend('sin','cos');
title('my plot');
print -dpng 'myPlot.png'
close;           % or,  "close all" to close all figs
figure(1); plot(t, y1);
figure(2); plot(t, y2);
figure(2), clf;  % can specify the figure number
subplot(1,2,1);  % Divide plot into 1x2 grid, access 1st element
plot(t,y1);
subplot(1,2,2);  % Divide plot into 1x2 grid, access 2nd element
plot(t,y2);
axis([0.5 1 -1 1]);  % change axis scale

%% display a matrix (or image) 
figure;
imagesc(magic(15)), colorbar, colormap gray;
% comma-chaining function calls.  
a=1,b=2,c=3
a=1;b=2;c=3;
```

## Control statements: for, while, if statement

```matlab
v = zeros(10,1)
for i=1:10,
    v(i)=2^i
end
% can also use "break" and "continue" inside for an while loops to control execution

i = 1;
while i <= 5,
    v(i) = 100;
    i = i+1;
end

if v(1)==1,
    disp('The value is one!');
elseif v(1)==2,
    disp('The value is two!');
else
    disp('The value is not one or two!');
end
```

## Functions

Example function:

```matlab
function y = squareThisNumber(x)

y = x^2;
```

To call the function in Octave,

- Navigate to the directory of the function functionName.m file and call the function 

```matlab
% Navigate to directory
cd /Users/username/Desktop/function folder

% call the function
functionName(args)
```

- Add the directory of the function to the load path and save it.


```matlab
% To add the path for the current session of octave
addpath('/Users/username/Desktop/function folder')

% To remember the path for future sessions of Octave, also do:
savepath
```

Octave's function can return more than one value:

```matlab
function [y1,y2] = squareandCubeThisNo(x)

y1 = x^2;
y2 = x^3;
```

Call the above function this way:

```matlab
[a,b] = squareandCubeThisNo(x)
```

## Vectorization

Vectorization is the process of talking code that relies on **loops** and converting it into **matrix operations**. It is more efficient, more elegant, and more concise.

With loops:

```matlab
prediction = 0.0;
for j = 1:n+1,
    prediction += theta(j)*x(j);
end;
```

With Vectorization:

```matlab
prediction = theta' * x;
```

## 参考链接

- [machine learning week2 lecture Notes -- coursera](https://www.coursera.org/learn/machine-learning/resources/QQx8l)
- [Octave Docs](https://www.gnu.org/software/octave/doc/interpreter/)
