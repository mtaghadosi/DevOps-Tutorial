# DevOps Answers 
#### This file contains all the necessary information’s about my decisions regarding to 5 questions that is in [instructions.md](instructions.md) file.
#### Answers by Mohammad Taghadosi ( Whole projects uploaded in [my GitHub](https://github.com/mtaghadosi/DevOps-Tutorial) )
#### This is not a complete solution, I tried to visualize the hole picture.

# Answers: (Every number refers to task numbers)
1.  For the first task, several conditions can be considered, all of which depend on many factors, such as product management strategies, the level of security required, the number of programmers involved and the number of rollouts in a period of time, the range of customers, and the product update and rollback strategy and even the considered budget for the entire project. I will try to present my ideal solution first without considering the factors I mentioned above and at the end I will mention other solutions as well. What is clear is that we need a web server! And according to the given instructions, we should use nginX. As DevOps, I suggest setting up this web server on the cloud. This cloud can be ECS on AWS or GKE on GCP or maybe we have an excellent data center in our company that we have already prepared with the best data center design standards and we can use the server-farm of that data center as the final force of our on-premises cloud.
