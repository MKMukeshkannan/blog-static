# Linear Regression with C++
In this blog, we are going to uncover the nitty-gritty details of linear regression and how it is implemented in a low-level language like C++. Exploring the wildreness is a way for you to escape the `abstraction hell` created by high-level languages like Python with these statements `model = LinearRegression()`. Lets jump right into the subject,




## what is linear regression???
> definition goes like,
> **linear regression is used to determine the best-fit line that accurately represents relationship between the independent variable and the dependent variable.**

```cpp
  for (int _ = 0; _ < 100000; ++_) {
    double error = 0;
    for (int i = 0; i < ages.size(); ++i) error += pow((hypothesis(ages[i]) - prices[i]), 2);
    error /= 2 * ages.size();

    double t1 = 0, t2 = 0;
    for (int i = 0; i < ages.size(); ++i) {
      t1 += (hypothesis(ages[i]) - prices[i]) * ages[i];
      t2 += (hypothesis(ages[i]) - prices[i]);
    };
    t1 = t1 / ages.size();
    t2 = t2 / ages.size();

    O1 -= 0.001 * t1;
    O2 -= 0.001 * t2;

    std::cout << error << '\n';
    epoch_list.push_back(_);
    error_list.push_back(error);
  };
```

