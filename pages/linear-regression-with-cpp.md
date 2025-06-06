# Linear Regression with C++
In this blog, we will uncover the nitty-gritty details of linear regression and how it is implemented in a language like C++. We will explore the wilderness — the way to escape **abstraction hell** created by languages like Python, with statements like `model = LinearRegression().` Let’s jump right into the subject.
<br />

## what is linear regression???
> definition goes like,
> **linear regression is used to determine the best-fit line that accurately represents relationship between the independent variable and the dependent variable.** 

### # In layman's terms,
Let us assume a classical linear regression example: you are trying to buy a house and need to estimate its price. Somehow, you have access to pre-sold house data in your area, and the data looks like this:
<br/>

| Age (years) | Price ($) |
|-------------|-----------|
| 1           | 25,000    |
| 1           | 22,000    |
| ...         | ...       |
| 10          | 10,000    |
| 11          | 10,500     |

<br/>
and on ploting it looks like this,

![plot](https://raw.githubusercontent.com/MKMukeshkannan/blog-static/refs/heads/main/static/lin-reg-data-plot.png)

From the graph, you can see a pattern: the price of the house is linearly dependent on the age of the house. That is, as the age increases, the price of the house decreases. (Why linear? Because it forms approximately a straight line.)

We know that a line's equation is given by,

> $$
> y = mx + c
> $$
> where, 
> - (y) - predicted value (price in this case)
> - (x) - input value     (age in this case)
> - (m) - slope value     (determines slope of the line)
> - (c) - constant value  (accounts for error)


We already have `y` and `x` from our dataset, so basically, linear regression helps us find the missing m and c that define the line.
<br />

The above example is a *highly simplified* and *not-so-practical* example designed not to scare you. Real-world datasets usually contain more than one independent variable, so the function won’t lie in a 2D plane anymore. The equation might be:
<br />

> $$
> f(x) = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + ... + \beta_n x_n
> $$
> where, 
> - ($f(x)$)    - predicted value (price in this case)
> - ($x$)     - independent-variable
> - ($\beta$) - co-efficients

<br />

## MEAN SQUARED ERROR
In order to train our model, we need a goal to achieve. For linear regression, the goal is to reduce the error.

### # **so, what do i mean by error?**
We have the dataset with the actual prices and the age of the houses (which we call $y$ and $x$). And we are trying to create an equation (in this case, a simple line) that fits the dataset. We first initialize the co-efficients with some random values. For example, the equation could be $\hat{y} = -22x + 900$ , where $\hat{y}$ is the predicted price value. 
For all the data points in the dataset, the deviation between $y$ and $\hat{y}$ is the error of our model (i.e., the equation).
<br />

### # **how do we summarize the error of the model**
The error for each data point might be different; that is, our model might predict a higher price value than expected, a lower price value than expected, or we might be accurate for certain data points. Therefore, we need a method to evaluate the error as a single value that summarizes the model’s accuracy. The most common method used to do so is *MEAN SQUARED ERROR*.
<br />

### # **THE formula**
It is a really simple algorithm. We just find the difference between the actual price and the predicted price, square it for all the data points, and then calculate the average of these errors.

> $$ MSE = J(m, c) =  \frac 1 {2n} \sum (\hat{y}_i - y_i)^2 $$
> *this function is also known as Cost Function*

Okay, now onto questions,

**_1 Why square? Why not just difference?_**
As i said earlier, the error could be both positive and negative (depending upon whether we've predicted above or below than actual value).
We also have MEAN ABSOLUTE ERROR that uses absolute function instead of square. And there is also ROOT MEAN SQUARE.

**_2 why we divide by 2n and not just n?_**
Just for convenience, later we will be differentiating the cost function. Having a 2 in the denominator gets canceled out during differentiation. You can use the formula without the 2 and still get the same result.

**_3 what do i mean by j(m, c)?_**
well, `m` and `c` are co-efficients in our equation. Later we need find derivative of the cost function to update the value of `m` and `c`. So, we will perform partial differentiation with respect to each of the co-efficients. For multi-variant equation we will have $J(\beta_1, \beta_2, ..., \beta_n)$ and we will have to find derivatives for each the co-efficients.
<br />

## Gradient Descent
it is an iterative method that will help us determine our `co-efficients` (in our case `m` and `c`) by minimising total error caused by current equation  ($y = mx + c$).
<br />

### # **THE GRADIENT DESCENT ALGORITHM**
the algorithm tells you to iteratively update new co-efficients, based on the following formula,
> $$ temp\_m = m - \alpha . \frac{\partial J(m, c)}{\partial m} $$\
> $$ temp\_c = c - \alpha . \frac{\partial J(m, c)}{\partial c} $$\
> $$ m = temp\_m $$\
> $$ c = temp\_c $$

How this thingy works if you ask, I want you to relay on mathematical instinct than getting deeper into the specifics for the next part.  Okay, You ready? \
How do you think the graph of our *cost function* will look like if we plot it for all possible `m` and `c`.
Yeah, that's right, it will look like a parabola. Consider the below graph,

![plot](https://raw.githubusercontent.com/MKMukeshkannan/blog-static/refs/heads/main/static/lin-reg-data-plot.png)

For a randomly initialized co-efficients we have an arbitrary error. Our goal is to update co-efficients such that the total error reaches its overall minimum. We know that derivative of a function gives slope at any point. Here, we don't actually care about the magnitude of the slope but its direction.\
For the above graph, the slope is **negative**, therefore, we will add a small value to the co-efficients hence moving the error towards the right, i,e towards the minimum.
Through the formula, through repeated iteration we will automatically move our co-efficients towards the minimum.
And that $\alpha$ is called the learning rate, that controls how quickly the model is trained. If $\alpha$ is too large, the parameter might overshoot and will never reach the minima, If $\alpha$ is too small, the training will take too much time to be trained. So, finding balance in learning rate is crucial for the training. Generally the $\alpha$ will be 0.1, do some experiments find your optimal training rate.

