---
layout: post
title: Coursera Machine Learning Week 5 - Neural Networks Learning
category: Academics
tags: Machine-Learning Coursera Neural-Networks
date: 2015-10-31
---

>'Talk is cheap. Show me the code.'  
>-- by Linus Torvalds.
<!--more-->

Originally, this article is for Group Presentation at LRZ Group Meeting, SJTU.

## Starting from Assignment
That's what we care about!

### Neural Networks Notations
![NN Notations](/assets/images/machine-learning/NN N1.png)

### Neural Networks Overview
![NN Overview](/assets/images/machine-learning/NN N2.png)

### NN Propagation
![Learning Algorithm Details](/assets/images/machine-learning/NN S3.png)

```matlab
a1 = X';
a1 = [ones(1, m); a1];

z2 = Theta1 * a1;
a2 = sigmoid(z2);
a2 = [ones(1, m); a2];

z3 = Theta2 * a2;
a3 = sigmoid(z3);
```

### NN Cost Function
#### without Regularization
![NN Cost Function](/assets/images/machine-learning/NN A1.png)

```matlab
Jc = zeros(num_labels, 1);
for c= 1:num_labels
  Jc(c) = (1 / m) * ( -log(a3(c, :)) * (y==c) - log(1 - a3(c, :)) * (1 - (y==c)));
end
J = sum(Jc);
```

#### with Regularization
![NN Cost Function](/assets/images/machine-learning/NN A2.png)

```matlab
Theta1R = Theta1(:, 2:end);
Theta2R = Theta2(:, 2:end);

nn_params_R = [Theta1R(:) ; Theta2R(:)];

J = J + (lambda / (2 * m)) * nn_params_R' * nn_params_R;
```

#### general Regularization
![NN Cost Function](/assets/images/machine-learning/NN N3.png)

### NN Gredient Calculation
#### Method
![Backpropagation Calculation](/assets/images/machine-learning/NN A4.png)

#### Detailed Backpropagation Calculations
##### Error
![Error Calculation for Output Layer](/assets/images/machine-learning/NN A5.png)

![Error Calculation for Hidden Layer](/assets/images/machine-learning/NN A6.png)

```matlab
for t = 1:m
  %delta3 = a3(:, t) - !([1:num_labels]' - y(t)); %not work for matlab, works for octave
  delta3 = a3(:, t) - not([1:num_labels]' - y(t));
  delta2 = Theta2R' * delta3 .* sigmoidGradient(z2(:, t));
end
```

##### Gredient
![Gradient Accumulation](/assets/images/machine-learning/NN A7.png)

```matlab
for t = 1:m
  Delta2 = Delta2 + delta3 * a2(:, t)';
  Delta1 = Delta1 + delta2 * a1(:, t)';
end
```

![NN Gradient](/assets/images/machine-learning/NN A8.png)

![NN Gradient Regularization](/assets/images/machine-learning/NN A9.png)

```matlab
D2 = (1 / m) * Delta2;
D1 = (1 / m) * Delta1;

%D2(:, 2:end) += (lambda / m) * Theta2(:, 2:end);
%D1(:, 2:end) += (lambda / m) * Theta1(:, 2:end);
D2(:, 2:end) = D2(:, 2:end) + (lambda / m) * Theta2(:, 2:end);
D1(:, 2:end) = D1(:, 2:end) + (lambda / m) * Theta1(:, 2:end);

Theta2_grad = D2;
Theta1_grad = D1;
```

## What's Next?
### Learning Algoritm Overview
![Learning Algorithm](/assets/images/machine-learning/NN S4.png)

### Learning Algorithm Procedures
![Learning Algorithm Procedures](/assets/images/machine-learning/NN S2.png)

### How to check Gredient
![Check Gradient](/assets/images/machine-learning/NN S5.png)

### Zero Initialization Problem
![Zero Initialization Problem](/assets/images/machine-learning/NN S6.png)

##So, that's All.
In fact, not all!

##References
[Matlab Function: Logical Operations](http://kr.mathworks.com/help/matlab/logical-operations.html)  
[Matlab Function: not](http://kr.mathworks.com/help/matlab/ref/not.html)  
[Matlab Function: logical](http://kr.mathworks.com/help/matlab/ref/logical.html)  
[Matlab Function: repmat](http://kr.mathworks.com/help/matlab/ref/repmat.html)  
[Matlab Function: Random Integers](http://kr.mathworks.com/help/matlab/math/random-integers.html)  
[Stackoverflow: shuffle matrix element in matlab](http://stackoverflow.com/questions/13257043/shuffle-matrix-element-in-matlab)  
[Matlab Central: How do I call MATLAB from the DOS prompt?](http://www.mathworks.com/matlabcentral/answers/102082-how-do-i-call-matlab-from-the-dos-prompt)  
