---
layout: post
title: "Machine Learning: Linear Regression with Multiple Variables"
date: 2014-07-17 15:54:00
tags: ["Machine learning", "Algorithms"]
thumbnail: /assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img_scatter.png
---

_Originally posted on my [old blog](https://rishabhroy.blogspot.com/) on 2014-07-17._

# 

  


[![](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img_scatter.png)](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img_scatter.png "Machine Learning: Linear Regression With Multiple Variables")

In this series of blog posts that I want to put up, I do not wish to get into all the theoretical details of the algorithms. I am working through the Machine Learning course offered by Coursera and want to record the implementation details. Since I’m new to both machine learning and octave, there could be some blatant errors. I’d very much like to correct them as I continue learning.

Let’s start off with the Linear Regression. The main modules that need to be implemented in any regression are the cost function and the parameter values. The cost function for a linear regression is defined as:

![J\(\\theta\) = \\frac{1}{2m}\\sum_{i=1}^{m}\(h_{\\theta}\(x^{\(i\)}\) - y^{\(i\)}\) ^2](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img1.png)

where the hypothesis ![h_{\\theta}\(x^{\(i\)}\)](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img2.png) is given by the linear model

![h_{\\theta}\(x^{\(i\)}\) = \\theta^{T}x = \\theta_{0} + \\theta_{1}x](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img3.png)

Note that the parameters of our model are the ![\\theta_{j}](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img4.png) values. These are the values that need to be adjusted to minimize the cost ![J\(\\theta\)](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img5.png). One way to do this is to use the batch gradient descent algorithm. In batch gradient descent, each iteration performs the update:

![\\theta_{j} := \\theta_{j} - \\alpha\\frac{1}{m}\\sum_{i=1}^{m}\(h_{\\theta}\(x^{\(i\)}\) - y^{\(i\)}\)x_{j}^{\(i\)}](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img6.png)

The update happens simultaneously for all j. With each iteration of gradient descent the parameters ![\\theta_{j}](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img7.png) come closer to the optimal values that will achieve the lowest cost ![J\(\\theta\)](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img8.png).

Instead of iterating, we could use vectorization to solve the problem quicker. The normal equation is applied in this case:

![\\theta = \(x^{T}x\)^{-1}x^{T}y](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img9.png)

One disadvantage of using the normal equation is that it is slower compared to the iterative method when the number of features become something like a million.

Let us compute the cost first. Say we have a matrix  _X_ which contains the training examples. Let there be  _m_ training examples and  _n_ features. Our matrix  _X_ will have one example in each row, thus having  _m_ rows. Also let us add 1 before the first column to each row of  _X_. Thus the dimensions of  _X_ will be ![m X \(n+1\)](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img10.png). Let  _y_ be the output vector of size ![m X 1](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img11.png). Finally, our ![\\theta](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img12.png) will be a ![\(n + 1\) X 1](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img13.png) vector denoting the parameters of our regression model.

Here’s the octave function to compute the cost of using the given ![\\theta](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img14.png) parameters.

12345678910111213| 
    
    
    function J = computeCost(X, y, theta)
    
    %COMPUTECOST Compute cost for linear regression
    
    %   J = COMPUTECOST(X, y, theta) computes the cost of using theta as the
    
    %   parameter for linear regression to fit the data points in X and y
    
    
    
    % Initialize some useful values
    
    m = length(y); % number of training examples
    
    
    
    % We need to return the following variable
    
    
    
    J = sum((X * theta - y) .^ 2) / (2*m)
    
    
    
    end
      
  
---|---  
  
[view raw](https://raw.githubusercontent.com/rishabhsixfeet/machine-learning-coursera/master/ex1/computeCost.m)[computeCost.m](https://github.com/rishabhsixfeet/machine-learning-coursera/blob/master/ex1/computeCost.m) hosted with ❤ by [GitHub](https://github.com/)

In the above code, ![X * \\theta](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img15.png) computes the value of ![h_{\\theta}\(x^{\(i\)}\) = \\theta_{0} + \\theta_{1}x](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img16.png). We then subtract the value of  _y_ for that training example. This value is then squared, summed and subsequently reduced by a factor of 2m.

The value of cost function can be used for debugging the iterations when the iterative batch gradient algorithm is used. A continuous reduction of the cost is a sign that the gradient descent is working properly. If the cost rises continuously instead then there is some error in the implementation – perhaps the ![\\alpha](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img17.png) value is too large?

Now that we have computed the cost, let us compute the parameters.

12345678910111213141516171819| 
    
    
    function [theta, J_history] = gradientDescent(X, y, theta, alpha, num_iters)
    
    %GRADIENTDESCENT Performs gradient descent to learn theta
    
    %   theta = GRADIENTDESENT(X, y, theta, alpha, num_iters) updates theta by 
    
    %   taking num_iters gradient steps with learning rate alpha
    
    
    
    % Initialize some useful values
    
    m = length(y); % number of training examples
    
    J_history = zeros(num_iters, 1);
    
    
    
    for iter = 1:num_iters
    
    
    
    theta = theta - (X' * (X * theta - y)) * (alpha / m);
    
    
    
    % Save the cost J in every iteration    
    
    J_history(iter) = computeCost(X, y, theta);
    
    end
    
    end
      
  
---|---  
  
[view raw](https://raw.githubusercontent.com/rishabhsixfeet/machine-learning-coursera/master/ex1/gradientDescent.m)[gradientDescent.m](https://github.com/rishabhsixfeet/machine-learning-coursera/blob/master/ex1/gradientDescent.m) hosted with ❤ by [GitHub](https://github.com/)

Note that in our solutions we have assumed that  _X_ is a matrix with unspecified number of columns. The above solutions can be applied no matter the number of features.

One important step when running the gradient descent is feature normalization. When there is a huge variation in the values that two features can take, then our gradient descent could take a long long time. One way to avoid it is to normalize the each feature by subtracting it’s mean from each value and dividing by the standard deviation or the range of that feature. Here’s an octave function to normalize features:

1234567891011121314151617181920212223242526272829303132| 
    
    
    function [X_norm, mu, sigma] = featureNormalize(X)
    
    %FEATURENORMALIZE Normalizes the features in X 
    
    %   FEATURENORMALIZE(X) returns a normalized version of X where
    
    %   the mean value of each feature is 0 and the standard deviation
    
    %   is 1. This is often a good preprocessing step to do when
    
    %   working with learning algorithms.
    
    
    
    % Initialize some useful values
    
    X_norm = X;
    
    mu = zeros(1, size(X, 2));
    
    sigma = zeros(1, size(X, 2));
    
    
    
    % For each feature dimension, compute the mean
    
    % of the feature and subtract it from the dataset,
    
    % storing the mean value in mu. Next, compute the 
    
    % standard deviation of each feature and divide
    
    % each feature by it's standard deviation, storing
    
    % the standard deviation in sigma. 
    
    %
    
    % Note that X is a matrix where each column is a 
    
    % feature and each row is an example. We need 
    
    % to perform the normalization separately for 
    
    % each feature. 
    
    
    
    mu = sum(X) / length(X);
    
    sigma = std(X);
    
    
    
    for i=1:length(X),
    
    X_norm(i,:) = (X(i,:) - mu) ./ sigma;
    
    end
    
    end
      
  
---|---  
  
[view raw](https://raw.githubusercontent.com/rishabhsixfeet/machine-learning-coursera/master/ex1/computeCost.m)[featureNormalize.m](https://github.com/rishabhsixfeet/machine-learning-coursera/blob/master/ex1/computeCost.m) hosted with ❤ by [GitHub](https://github.com/)

In the iterative gradient descent algorithm, one has to guess an ![\\alpha](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img18.png) and also decide upon the number of iterations. It might take multiple runs of the algorithm to settle down on a suitable value for the above variables. Using the normal equation obviates the need to guess these values. It computes the value of ![\\theta](/assets/img/migrated/machine-learning-linear-regression-with-multiple-variables/img19.png) in one shot. The code for the same is:

12345678| 
    
    
    function [theta] = normalEqn(X, y)
    
    %NORMALEQN Computes the closed-form solution to linear regression 
    
    %   NORMALEQN(X,y) computes the closed-form solution to linear 
    
    %   regression using the normal equations.
    
    
    
    theta = inverse(X' * X) * X' * y;
    
    
    
    end
      
  
---|---  
  
[view raw](https://gist.github.com/anuvrat/2795580/raw/normalEqn.m)[normalEqn.m](https://gist.github.com/anuvrat/2795580#file-normaleqn-m) hosted with ❤ by GitHub
