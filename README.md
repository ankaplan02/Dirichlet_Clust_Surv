# Dirichlet_Clust_Surv
These sets of codes comprise a pair of models that first cluster subjects based on longitudinal measures taken over the course of a study. One may specify one or multiple longitudinal trajectories to base the clustering on for the subjects. Second, using the cluster labels, it evaluates the probability of an event (e.g., therapy drop-out) at each time-point (e.g., therapy session) based on fixed covariates and a session-cluster specific effect. 
Couple of items need to be specified before its use:

(A) For Clustering subjects based on multiple longitudinal trajectories: 

(1) X: fixed covariate matrix in long format (multiple rows per subject) that comprises the trajectory to be assumed! (we assume quadratic, i.e., {1, t, t^2})

(2) y: longitudinal outcome matrix same number of rows as X but number of columns can be more than 1

(3) G: number of unique subject labels 

(4) n_s: the index number for each subject's first observation in the array y

(5) n_i: the index number for each subject's final observation in the array y 
    
(6) n_a: each subject's total number of observations in the array y

(7) Mtrunc: number of latent mixture components (we set ours to 20)

(8) dp.alp: the concentration parameter (alpha) value, we set ours to be 3/log(G)

(9) Dist_Mat: a distance matrix with dimension T by T, with T being the max number of observations a subject can have (e.g., T=8); entries we used were (t_i - t_j)^2 for the exponential covariance matrix 

(10) D2: identity matrix of size T

(11) indics: a matrix with number of rows equal to number of columns of X, and number of columns equal to the number of columns of X - 1. 

     To specify the prior precision matrix of the DP latent effect vector, in JAGS we need to be a little tricky. 
     
     indics[1,] refers to all numbers not equal to the row index 1: (2, 3)
     
     indics[2,] refers to all numbers not equal to the row index 2: (1, 3)
     
     and so on. 
     
 This enables specifying off diagonal entries of the prior precision of the DP atoms to be zero within the JAGS code
 
 
 (B) For using their post processed clusters as time-point specific effects in a drop-out model (survival)
 
 (1) y2: N x J matrix; where N is number of unique subjects and J is number of time points 
 
 (2) X: N x p matrix; where p is number of fixed effects (clinical markers)
 
 (3) clusts: a length-N vector of post-processed cluster assignments based on the clustering used above
 
 Using the latent cluster assignments in (A), we need to process them using any method that can find an optimal configuration of subjects into groups based 
 on posterior samples of cluster labels. We use the 'mcclust' R package and the 'minbinder' function on the posterior similarity matrix evaluated with the posterior
 cluster labels in (A). We first used Lau-Green but computation time was much too long, so we then started using the 'average' method. 
 
 
