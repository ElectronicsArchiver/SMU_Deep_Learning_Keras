---
title: "Neural Network"
teaching: 20
exercises: 0
questions:
- "How to use Neural Network in Machine Learning model"
objectives:
- "Learn how to use ANN in ML model"
keypoints:
- "ANN"
---

## Neural Network
![image](https://user-images.githubusercontent.com/43855029/114472746-da188c00-9bc0-11eb-913c-9dcd14f872ac.png)
![image](https://user-images.githubusercontent.com/43855029/114472756-dd137c80-9bc0-11eb-863d-7c4d054efa89.png)

- Formulation of Neural Network
![image](https://user-images.githubusercontent.com/43855029/114472776-e997d500-9bc0-11eb-9f70-450389c912df.png)
```
Here, x1,x2....xn are input variables. w1,w2....wn are weights of respective inputs.
b is the bias, which is summed with the weighted inputs to form the net inputs. 
Bias and weights are both adjustable parameters of the neuron.
Parameters are adjusted using some learning rules. 
The output of a neuron can range from -inf to +inf.
The neuron doesn’t know the boundary. So we need a mapping mechanism between the input and output of the neuron. 
This mechanism of mapping inputs to output is known as Activation Function.
```
![image](https://user-images.githubusercontent.com/43855029/114472956-485d4e80-9bc1-11eb-9db8-19072c5ea8ba.png)
![image](https://user-images.githubusercontent.com/43855029/114472972-51e6b680-9bc1-11eb-9e78-90ec739844ee.png)
![image](https://user-images.githubusercontent.com/43855029/114472978-557a3d80-9bc1-11eb-9be3-7f49a54850cc.png)

- Type of Neural Network:
![image](https://user-images.githubusercontent.com/43855029/114473007-5f9c3c00-9bc1-11eb-9923-7be372bd23f5.png)

### Implementation
```r
install.packages("neuralnet")
```
Split the data
```r
library(caret)
library(neuralnet)

datain <- mtcars
set.seed(123)
#Split training/testing
indT <- createDataPartition(y=datain$mpg,p=0.6,list=FALSE)
training <- datain[indT,]
testing  <- datain[-indT,]
#scale the data set
smax <- apply(training,2,max)
smin <- apply(training,2,min)
trainNN <- as.data.frame(scale(training,center=smin,scale=smax-smin))
testNN <- as.data.frame(scale(testing,center=smin,scale=smax-smin))
```

- Fit the Neural Network using **10** hidden layer:
```r
set.seed(123)
ModNN <- neuralnet(mpg~cyl+disp+hp+drat+wt+qsec+carb,trainNN, hidden=10,linear.output = T)
plot(ModNN)
```
![image](https://user-images.githubusercontent.com/43855029/114492632-f0d1d980-9be6-11eb-89c5-196f9f3546d9.png)
```r
#Predict using Neural Network
predictNN <- compute(ModNN,testNN[,c(2:7,11)])
predictmpg<- predictNN$net.result*(smax-smin)[1]+smin[1]
postResample(testing$mpg,predictmpg)
     RMSE  Rsquared       MAE 
3.0444857 0.8543388 2.3276645 
```