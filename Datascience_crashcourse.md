# KNN

KNN stands for K nearest neighbour. It is one of the machine learning models where you look at similar data to the data you have and use it to predict the values on your data compared to that of the model.

There are many others such as decision trees and random decision forest, linear line regression and many more. Each one will be useful in certain circumstances with the data available. Take house prices - if we have data mainly on moderate houses (3 bedrooms, 1 toilet etc.) then we wouldn't want to use KNN because the when we have a mansion which has 12 bedrooms, 12 toilets etc, the nearest data we will find will be based around that of the moderate houses, so if we were to predict house prices of mansions using KNN by modelling normal houses it would tell us the should cost roughly the same - where linear line regression would extrapolate that the extra rooms and bathrooms would mean a larger price.


## Creating a KNN Solution
### Task 1: Given two points work out the distance between them (Pythagorean Theorem)

Given two points A and B, which have there own separate X and Y coordinates, work out the difference between them
i.e. 
```python
a_x = 3
a_y = 8
b_x = 4
b_y = 2
```

### Task 2: Given two points work out the distance beween them
Given two points A and B, which have there own separate X and Y and Z coordinates, work out the difference between them
i.e. 
```python
a_x = 3
a_y = 8
a_z = 4
b_x = 4
b_y = 2
b_z = 9
```

### Task 3: Given two points work out the distance beween them
Given two points A and B, which have there own separate X and Y and Z and T and S coordinates, work out the difference between them
i.e. 
```python
a_x = 3
a_y = 8
a_z = 4
a_t = 1
a_s = 22
b_x = 4
b_y = 2
b_z = 9
b_t = 3
b_z = 29
```

## How does KNN work
KNN works by looking at the nearest neighbours. The number of neighbours you look at it determined by K. A higher K value will average out the spread of data into a neat split line (however can over average values) and  a lower K value will be too granular. 

Usually sqrt(number_of_rows) = k 

This can change the value:

![knn](data:image/png;base64,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)

Here a K value of 3 would categorise it as RED, and a K value of 5 would categorise the centre as BLUE.

### How to do KNN programmatically.
We just saw that based on the nearest neighbours we can predict the incoming value. So given we can calculate the distance between two points given a load of data between them, if we had a table full of data for a point and we needed to determine a single value on it, we could have it run over the other points to calculate a distance to each of them, take the nearest K number of points and see what they are classified as. 

Think of a table rather than the X,Y,Z,T,S exercise:

| Particle  | X | Y  | Z  | T  | S  | Colour   |
|---|---|---|---|---|---|---|
| A  | 3  | 8  | 4  | 1  | 22  | RED   |
| B  | 4  | 2  | 9  | 3  | 29  | BLUE
| C  | 2  | 5  | 2  | 5  | 34  | RED
| D  | 3  | 6  | 5  | 4  | 30  | RED
| E  | 5  | 2  | 2  | 5  | 34  | BLUE
| F  | 4  | 6  | 5  | 4  | 30  | RED
| G  | 5  | 2  | 3  | 5  | 34  | BLUE

You will have to look at normalising data (as each column should have equal weightings (unless using weighted KNN)) 

Create a new particle with X,Y,Z,T,S coordinates with a KNN of 3 and see what it will be classfied as (do it by hand)

If you can do it by hand convert it too python

Once you have done it with this exercise, look for a cancer dataset (I believe a breast cancer dataset is available somewhere) with classifications of cancerous and non cancerous. Cut the dataset in half, and use half to TRAIN, and then use the trained model to TEST the other half to see whether you are getting it correct.

