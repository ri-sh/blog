---
layout: post
title: "MNIST handwritten digit recognition"
date: 2015-05-09 11:37:00
tags: ["Nueral Networks", "Machine learning"]
---

_Originally posted on my old blog on 2015-05-09._

#  MNIST handwritten digit recognition[¶](https://draft.blogger.com/blogger.g?blogID=400547060648725706#MNIST-handwritten-digit-recognition)

I wanted to try and compare a few machine learning classification algorithms in their simplest Python implementation and compare them on a well studied problem set. The MNIST dataset is a set of images of hadwritten digits 0-9. The challenge is to find an algorithm that can recognize such digits as accurately as possible. More details can be found on [Kaggle](http://www.kaggle.com/c/digit-recognizer), or at http://yann.lecun.com/exdb/mnist/index.html.  
I was almost able to do this using scikit-learn exclusively, but I really wanted to include a simple neural network, and there doesn't seem to be any supervised neural network algorithms built into scikit-learn currently. For the neural network, I decided to use [nolearn](https://pythonhosted.org/nolearn/) for it's relative simplicity.  


In [22]: 
    
    
    import pandas as pd
    import numpy as np
    from sklearn import cross_validation
    from sklearn.ensemble import RandomForestClassifier
    from sklearn.svm import LinearSVC
    from sklearn.linear_model import SGDClassifier
    from sklearn.neighbors import KNeighborsClassifier
    from sklearn.metrics import accuracy_score
    from nolearn.dbn import DBN
    import timeit
    

As the dataset is rather large (4200 entries), I set aside only 1/10th for cross validation.  


In [28]: 
    
    
    train = pd.read_csv("train.csv")
    features = train.columns[1:]
    X = train[features]
    y = train['label']
    X_train, X_test, y_train, y_test = cross_validation.train_test_split(X/255.,y,test_size=0.1,random_state=0)
    

##  Random Forest

My first choice was a random forest algorithm. I like random forests because they are so versatile and require so little tuning. I was quite surprised out how quickly I was able to get very good results. This ran in only a few seconds.  


In [29]: 
    
    
    clf_rf = RandomForestClassifier()
    clf_rf.fit(X_train, y_train)
    y_pred_rf = clf_rf.predict(X_test)
    acc_rf = accuracy_score(y_test, y_pred_rf)
    print "random forest accuracy: ",acc_rf
    
    
    
    random forest accuracy:  0.937142857143
    
    

##  Stochastic Gradient Descent

My next choice was to try stochastic gradient descent, as it is popular for large-scale learning problems and is known to work efficiently. I used all the default parameters. In particular, the loss function defaults to 'hinge', which gives a linear SVM. This algorithm also runs in only a few seconds. The accuracy is not as high as the random forest, but still respectable.  


In [34]: 
    
    
    clf_sgd = SGDClassifier()
    clf_sgd.fit(X_train, y_train)
    y_pred_sgd = clf_sgd.predict(X_test)
    acc_sgd = accuracy_score(y_test, y_pred_sgd)
    print "stochastic gradient descent accuracy: ",acc_sgd
    
    
    
    stochastic gradient descent accuracy:  0.893095238095
    
    

##  Support Vector Machine

For comparison, I thought it would be intersting to try a 'non-stochastic" SVM. This one is significantly slower than the SGD method above (about a minute) and only seems to provide a minor improvement in accuracy.  


In [36]: 
    
    
    clf_svm = LinearSVC()
    clf_svm.fit(X_train, y_train)
    y_pred_svm = clf_svm.predict(X_test)
    acc_svm = accuracy_score(y_test, y_pred_svm)
    print "Linear SVM accuracy: ",acc_svm
    
    
    
    Linear SVM accuracy:  0.91
    
    

##  Nearest Neighbors

I had read that Nearest Neighbors had been successful on handwritten digit classification and I noticed that it was discussed in the Kaggle forum for this problem, so I decided to try it. It is much slower than the algorithms above, but is indeed quite accurate.  


In [38]: 
    
    
    clf_knn = KNeighborsClassifier()
    clf_knn.fit(X_train, y_train)
    y_pred_knn = clf_knn.predict(X_test)
    acc_knn = accuracy_score(y_test, y_pred_knn)
    print "nearest neighbors accuracy: ",acc_knn
    
    
    
    nearest neighbors accuracy:  0.966666666667
    
    

##  Neural Network

As many of the most accurate published algorithms for this problem employ some sort of neural network, I wanted to try at least one implementation. I used the [nolearn](https://pythonhosted.org/nolearn/) package, choosing my parameters based on the [example](https://pythonhosted.org/nolearn/dbn.html#example-mnist) in their documentation. The speed was comparable to the nearest neighbors implementation above, and it was slightly more accurate.  


In [39]: 
    
    
    clf_nn = DBN([X_train.shape[1], 300, 10],learn_rates=0.3,learn_rate_decays=0.9,epochs=15)
    clf_nn.fit(X_train, y_train)
    acc_nn = clf_nn.score(X_test,y_test)
    print "neural network accuracy: ",acc_nn
    
    
    
    neural network accuracy:  0.977142857143
    
    

##  Verdict

My overall impression is that neural networks have the most promise in terms of accuracy for this problem, while a simple, straigtforward random forest provides the best balance of accuracy and efficiency.  


In []:
