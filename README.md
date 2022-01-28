# Cloud_resource_allocation_using_machine_learning
A Resource allocation application using Machine learning technique Assisted by Cloud Computing implemented in Python

## 1 ***Introduction***
The following picture depicts the geographic system over network.

<img style="display:block;margin-left: auto; margin-right: auto; width:50 %" width="500px" src="figure 1.png">


## 2. ***Data***
#### 2.1 Description
As the system uses Machine learning to allocate the proper resources, we need data. But finding prepared and clean data for doing such a thing on a small scale is not much possible. So we created my own Data. The Data Creator module uses a predefined template to create data. To execute the seeder and create the data, we will now run the seeder.py file. Now, the data is created so we can move on to understand the cloud module.

#### 2.2 Process
First the seeder module reads the seeder_data_template.csv file and creates a data based on it in /Cloud/storage/data/dat.csv file.
Attributes, classes and values are manipulatable but needs coding also.

#### 2.3 Execution
To execute the seeder and create the data, run `python3 Cloud/seeder.py`. change the '/' to '\\' in case you are using a non-unix based system.
the command outputs the data.csv file

## 3. ***Cloud Module***

#### 3.1 Description
The Cloud Module is responsible for storing raw data, preprocessing the data, managing the warehouses, learning based on the data and creating an ML model based on it, and finally sending the model to edges. we can consider it as the brain of our system. By comparison, we found the decision tree to work best with our data.

#### 3.2 Process
Data is stored in it's storage system and in here, we do the whole learning process in the cloud.py module (in Cloud/Storage/cloud.py, Not a Python Class). the code is fully commented in Docstring format so there is no need for details description but an overview. the Module reads the Data, splits it on 70 percent for training and 30 percent testing, Trains it's model using Decision Tree Classifier Technique, Tests it's model and persists it's model in the storage system.
The Classifier Technique is chosen using Cross validation Technique which showed that Decision Tree can classify the Data with 85 percent Accuracy which is a good number for resource allocation. other techniques like KNN had about 65 percents accuracy on our data.

#### 3.3 Execution
The cloud module is executable using `python3 Cloud/cloud.py` command. it outputs a model with joblib format on our storage.

## 4. ***Cloudlet***

#### 4.1 Description
Cloudlets are just Tasks, a name chosen by Cloudsim framework. Tasks are generated on different areas (in our application we have 3 different position).

Attributes, values and classes are listed bellow. the raw Data Created are all in numbers categorizes or continues, but we preprocess them to categorized values, for example a task with 120 million is considered as a task with high level instruction (more than 100). the thresholds are calculated using test and trial process.
values af each tasks are Generated randomly.
***What we need to do is to find best Worker for Each Task***

#### 4.2 Process
Cloudlet is a Class Generated in edges so we placed it in Edges/Cloudlet.py. creating object of the Class initializes it's attributes. Code is fully commented.

## 5. ***Worker Node***

#### 5.1 Description
Worker is our Resources, machines which does the jobs. all workers are Inheriting the Mother Worker Class (Edges/workers/worker.py file) which has all the functionalities and attributes. each worker can override these properties by their own.
Our application has 3 workers. each worker has some attributes:

<ul>
  <li>
    Position: position of the worker server
  </li>
  <li>
    Power: Power of the worker server in million instructions per seconds
  </li>
  <li>
    Bandwidth: Bandwidth Between the worker server and other Workers in MB/s
  </li>
  <li>
    Makespan (timer): the time the worker consumed to do the jobs in seconds
  </li>
</ul>

#### 5.2 process
Worker is a class, so creating a class with optional name and inheriting the Worker Class would create a worker server.

## 6. ***Master Node***

#### 6.1 Description
If we name the Cloud module as the Brain of our system, Master Node of the system is it's heart. It's responsible for getting information of each Cloudlet and finding the best Worker for it using the trained model in the cloud storage. for instance web have a task, the metadata of the task is being sent to the Master Node, it choses best Worker by considering all attributes of Cloudlet and Workers, then Attaching The Cloudlet To The Worker. Worker then gets the Cloudlet through the Network and does the job.
In this application we've implemented a ***greedy approach*** to compare the functionality and performance of the ML model with it. the Greedy algorithm considers Size and Instructions of the Task not it's priority.

#### 6.2 execution
Master Class declares itself and does the job. following command: `python3 Edges/Master.py` would do it all and prints the outputs.

## ***Conclusion***

We chose values for attributes in Cloudlets and workers using Test and Trial. Our purpose was that the Worker 3 have more power
than Worker 2 and Worker 2 than 1. but the bandwidth between 1 and 3 is very low , between 1 and 2 is high and between 2 and 3 is the highest.
Running the resource allocation process using the application had following results average on 10 times of execution:

Using Machine learning Technique:
<ul>
    <li>
      Worker 1 Makespan:  221.5 s
    </li>
    <li>
      Worker 2 Makespan:  569 s
    </li>
    <li>
      Worker 3 Makespan:  982.5 s
    </li>
</ul>

Using Greedy Algorithm
<ul>
    <li>
      Worker 1 Makespan:  322.2 s
    </li>
    <li>
      Worker 2 Makespan:  248.4 s
    </li>
    <li>
      Worker 3 Makespan:  1050.5 s
    </li>
</ul>

Which shows the ML model does a better job spreading the tasks on workers and lowering the overall makespan.
As you can see the Greedy algorithm Sends most of the Tasks To Worker 3. but ML balances the loads as we wanted it to be.
