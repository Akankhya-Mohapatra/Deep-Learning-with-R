Most of data science projects consists of working with relatively large datasets. Whether it is during data exploration or while building regression models, parallel computation can almost always help to speed up processes.

The best way to understand a parallel task in layman terms is to compare it with a real-life situation. We can take as an example a school project where the final task is to present a 30 page report. One way of tackling this job is by doing every individual part of the report (introduction, methodology, analysis, etc.) in a sequential manner (by the same person) meaning the total time required will be the sum of all the individual tasks duration. This method is fairly simple. Although it does not demand much coordination, it is achieved at the cost of slow performance and not fully utilizing all its ressources (only 1 student in the team is working). 

However, we can use a methodology that is much more effective when dealing with big projects by separating tasks between 3 students in a parallel way. Each student works individually on one task of the assignment with the objective to combine their work at the end. The total length of the project will be dependent on the longest task duration as well as the coordination time of combining all the work together. 

In technical terminology, the coordination time is called the *overhead time* and the student can be represented as clusters, workers, cores or threads.

Another important factor in a parallel task consists of a good distribution of jobs. For a parallel task to be fully optimized,each core needs to be assigned a similar length else there will be a bottleneck effect and many cores will patiently wait for their next task, wasting precious resources.

Finally, when building a parallel task, we need to specify how to combine the information gathered by every worker. In R,most of the parallel libraries let us decides the way to combine all the tasks output. For example, we can receive the information as a list, as matrix, as a vector or even as an aggregation of all the different outputs. 

The various libraries used in this project for parallelism are - foreach, doParallel, Parallel. In order for Neural networks to do heavy calculation, we use it along with GPU to efficiently complete such tasks in lesser time duration. GPUs are expensive tools and hence we have written an R code to access the GPU feature and run it in the stable environment of Google Colab. We begin with Monte Carlo simulation and compare the running time for each tasks in parallel. Here, we find that parSapply yields much faster result than foreach in sequence or parallel run. 

Next, we have used a dota2 games dataset from UCI Machine Learning Repository (Dua and Graff 2017) to determine the trio of heroes that was picked most often in same team across all games. The sequential running of this operation took almost 8 minutes to run. We try to minimize this duration using parallel computation. Not surprisingly, we obtain much better results with foreach and Mclapply function. We do einsum operations for matrix calculations on the dataset using torch library (available in almost all matrix/tensor libraries like Tensorflow or Torch (or numpy in python)). The best explanation of this operation can be found at : https://www.tensorflow.org/api_docs/python/tf/einsum https://www.tensorflow.org/api_docs/python/tf/einsum. As a result, we see a huge difference between the sequential and all the parallel methods. We also see that the transformation of the problem into tensor computation had a very significant impact on the processing speed. Using a GPU provides the best results.

As our next task in this project, we aim at improving computation in ensemble methods using parallel run. Since models like random forests are based on the aggregation of multiple trees and since each of those models do notdependent on each other during their creation, process like random forests can greatly be improved by parallelcomputing. We  assess the performance of multiple R packages that allow us to create random forests models. Using the Dota2 dataset that we used previously , we add the information about game types and game categories to the hero information. In each random forests, we try to predict in each game if if the blue team would have win the game.Nowadays, predicting the outcome of a game could be very useful. It can be useful when you are picking your hero if you want to know which one would increase your chance of winning. It can also be useful when beting on esports (online gaming), since this kind practice is becoming more and more common.

We see that the package ranger seems to be the best in all cases and that it is resilient to the augmentation of number of trees. All in all, it is interesting to see that the relation ship between the number of trees and the calculation time is linear andthat the best package to do a random forest in term of execution speed seems to be the ranger package followed by the simple doPar method coupled with the random forest package. In the end, it is very impressive to see the performance delivered by the ranger package, since it increased the speed of the random forest creation by fold. In some cases, we saw that the randomForestSRC gave slower performance (even slower than the sequential methods) when the numberof trees where very low. However, when increasing the number of trees, it becomes more efficient than the sequential method.

### There 2 are important lessons to hold from this work.
### 1. Before starting a project and going with parallelization, one musthave a good idea of how to streamline and simplify a problem. By using the right libraries and the right operations, plentyof minutes could be save. 
### 2. Parallelization is not meant for small problem. Overhead time and compilation could eat thegain made from parallelizing task.
