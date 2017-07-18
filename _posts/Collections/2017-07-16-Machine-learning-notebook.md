---
layout: post
title: "Machine Learning Introduction"
description: ""
category: Machine Learning
tags:  [Machine Learning]
---
{% include JB/setup %}

# What is Machine Learning

## Informal definition

The field of study that gives computers the ability to learn without being explictly programmed. - by Arthur Samuel

## A modern definition

A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E. - by Tom Mitchell.

#### Example:
E = the experience of playing many games of checkers

T = the task of playing checkers.

P = the probability that the program will win the next game.

# Machine Learning Algorithms

In general, there are two broad classifications:
+ Supervised learning
+ Unsupervised learning

Others:
+ Reinforcement learning
+ recommender systems

# Supervised Learning

We are given a data set and already know what our correct output should look like, having a relationship between the input and output.

There are two categories of supervised learning problems:
+ regression problems: we are trying to predict results within a continuous output, meaning that we are trying to map input variables to some continuous function.
+ classification: we are trying to predict results in a discrete output. Namely, map input variables into discrete categories.

Example:

(a) Regression - Given a picture of a person, we have to predict their age on the basis of the given picture

(b) Classification - Given a patient with a tumor, we have to predict whether the tumor is malignant or benign.

# Unsupervised Learning

Unsupervised learning allows us to approach problems with little or no idea what out results should look like. We can derive structure from data where we don't necessary know the effect of the variables.

We can derive this structure by clustering the data based on relationships among the variables in the data.

With unsupervised learning, there is no feedback based on the prediction results.

Example:
Clustering: Take a collection of 1,000,000 different genes, and find a way to automatically group these genes into groups that are somehow similar or related by different variables, such as lifespan, location, roles, and so on.

Non-clustering: The "Cocktail Party Algorithm", allows you to find structure in a chaotic environment. (i.e. identifying individual voices and music from a mesh of sounds at a cocktail party).

# Linear Regression

### Notations:
+ `x(i)`: input variables
+ `y(i)`: output or target variables that we are trying to predict.
+ `(x_i, y_i)`: training example
+ a list of training example: training set
+ `(i)`: index into the training set
+ X: space of input values
+ Y: spance of output values

### Supervised problem:
Given a training set, to learn a funciton `h: X -> Y` so that h(x) is a good predictor for the corresponding value of y. This function is called hypothesis.

### Regression problem vs classification problem
Regression problem: the target variable that we're trying to predict is continuous.
classification problem: y can take on only a small number of discrete values.
+ example: if given the living area, we wanted to predict if a dwelling is a house or an apartment

# Cost Function

We can measure the accuracy of our hypothesis function by using a cost function.

$$J(\Theta_0, \Theta _1) = \frac{1}{2m}\sum_{i=1}^{m}(h_{\Theta }(x_i) - y_i))^{2}$$

We should try to minimize the cost funciton.

(TODO: fix style/syntax)







