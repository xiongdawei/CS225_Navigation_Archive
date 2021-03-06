# RESULTS

## Dijkstra Algorithm

The Dijkstra Algorithm is a BFS algorithm to find the shortest path from one point to another. The logic behind Dijkstra algorithm is to divide all points into two parts: visited and unvisited. The whole algorithm consists of several steps:
1. check all points unvisited that's reachable from current visited points
2. find the nearest point of all points visited
3. update the estimate of that point (i.e the distance needed to get to that point from starting point)
4. add that point into the "visited" group
5. repeat step 1-4 until the destination is found.
The whole algorithm is divided into several functions: 
the constructor; the findnearestpoint function (to find the nearest of all points); the getpoints function (find all currently unvisited points of a point);

Improvements and analysis

The current algorithm can only tell the shortest estimate of one point to another, so from this algorithm we can't construct a visualization of the path. Instead we can only construct a graph of all nearest points collected (which in most case contains most points of the map). 

The algorithm itself can be further imrpoved with the getpoints function. Currently everytime getpoints function is called it will try to clear all connected points of a point and add them again according to the status of the point (visited or not). Due to the point struct, this is still in progress.

![image](dddd.png)
Visualization of nearest points on s1 dataset
--------------------------------------------

    Point* p3 = new Point(116.3115702, 39.9746749);
    Point* p4 = new Point(116.3128514, 39.9720423);
    Dijkstra star(p3, p4, "./tests/s1.csv");
    cout<<"the distance between points is: "<<p3->distance(p4);
    for(size_t i = 0; i < star.road.size(); i++){
         cout<<star.road[i]->x<<" "<<star.road[i]->y<<star.road[i]->best_estimate<<endl;
    }
    return 0;
some points are tested, and the estimates are shown here [link](https://github-dev.cs.illinois.edu/cs225-sp21/mqiu3-daweix3-hanbog2-lurenw2/blob/master/tests.txt).

## A* (Astar) algorithm

A star algorithm is by far the most popular algorithm in finding the shortest path from one point to another. The essence of it is BFS, but unlike BFS (Blind Search and it looks like wavefront)that will find the all the points surround the current point, A* algorithm has 'brain', which means that it will directly research towards the destination. It is achieved through calculating the cost of one point and the cost is composed of two parts. We call it heuristic search. In our algorithm design, there are three types of cost and they are calculated through the following function:



    double Astar::calcG(Point* A, Road* R) 
    {
    	if (A->parent==nullptr) return R->length_;
    	else return A->parent->G + R->length_;
    }
    
    // Actual operating cost having been already traversed
    
    double Astar::calcH(Point* A) 
    {
    	return destination_->distance(A)*1.33;
    };
    
    // Direct distance between A and B;

    double Astar::calcF(Road* road, Point* point) 
    {
    return calcG(point,road) + calcH(point);
    }
    // Sum of G and H
 
The time complexity of A* search depends on the heuristic functions. In our cases, it should be O(b^m) where heuristic is completely uninformative and he edge costs are all the same, just like BFS. The space complexity is also O(b^m) like BFS, A* maintains a frontier which grows with the size of the tree. 

 
During the testing of our algorithm, we found out that weight is a really important factor in our algorithm design. Essentially, heurisitc search like A* is informed search. We found out that if we multiply the weight of H with a different factor, the result will totally different.

![image](factor5.png)
When the factor is 5
--------------------------------------------
![image](factor1.33.png)
When the factor is 1.33
--------------------------------------------
Obviously, factor of 1.33 gives us better result generally speaking. But what cause this difference?



This informs us that our algorithm can be optimized in multiple ways since it relies on retrieving the next point with the lowest F value. In real life traffic, multiple factors can be considered. Is it a highway or pedestrian? Is this road in downtown or in suburb area? Does this road charges any fee? If the user is planning to go to work using our navigation algorithm, can we find not only the shortest path, but also the least time consuming path by bypassing some of the traffic-congested area? We then realize that modern map&nagivation application have some factors that needed to be taken into consideration. 

## Error Analysis
After calculating the shortest path, we also compare our navigating result with which from Google map. We also found some problems with our dataset that doesn't give us full information about the road and point. By looking at the picture of factor of 1.33, we found out that sometimes the navigation guides us to get out of the main road and go to the branches road and then come back to the main road. We think it is due to the complexity of the map and our algorithm is not that solid. Beijing is an ancient city and the roads are not strictly horizontal and vertical. I think the same will apply to European cities like London or Barcelona. Nevertheless, I do think that our algorithm can have a really good result on American style cities like Chicago or New York City where mostly all the major roads are in a horizontal or vertical direction. 
