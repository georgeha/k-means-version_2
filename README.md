k-means-version_2
=================


Hands-on Job Sumbimission:
In order to make this example work, we need first to install the following::
```
virtualenv $HOME/myenv
source $HOME/myenv/bin/activate
```
Install Radical-Pilot API:
```
pip install radical.pilot
```
Install MondoDB (only if you want to run locally):

Linux Users:
```
apt-get -y install scons libssl-dev libboost-filesystem-dev libboost-program-options-dev libboost-system-dev libboost-thread-dev
git clone -b r2.6.3 https://github.com/mongodb/mongo.git
cd mongo
scons --64 --ssl all
scons --64 --ssl --prefix=/usr install
```
Mac Users:
```
brew install mongodb
mkdir -p /data/db
chmod 755 /data/db
mongod
```
Finally, you need to download the source files of k-means algorithm:
```
curl -O https://raw.githubusercontent.com/georgeha/k-means-version_2/master/k-means.py -O https://raw.githubusercontent.com/georgeha/k-means-version_2/master/clustering_the_elements.py -O https://raw.githubusercontent.com/georgeha/k-means-version_2/master/finding_the_new_centroids.py -O https://raw.githubusercontent.com/georgeha/k-means-version_2/master/dataset4.in
```

Run the Code:
To give it a test drive try via command line the following command:
```
python k-means.py 3
```
where 3 is the number of clusters the user wants to create.

More About this algorithm:
This algorithm creates the clusters of the elements found in the dataset4.in file. You can create your own file or create a new dataset file using the following generator:
```
curl -O https://raw.githubusercontent.com/georgeha/k-means-algorithm/master/creating_dataset.py
```
run via command line:
```
python creating_dataset.py (number_of_elements)
```

The algorithm takes the elements from the dataset4.in file. Then, it chooses the first k centroids using the quickselect algorithm. It divides into number_of_cores files the initial file and pass each file as an argument to each Compute Unit. Every Compute Unit find in
which cluster every elemnent belogs to and creates k different sums of the elements coordinates. and returns this sum. Then, we sum
all the sums of the CUs, and find the avg elements who are closeset to the new centroids. Afterwards, we do the same decompotition, but
this time we try to find the new centroids. From each CU we find the nearest element to each centroid, and return them to the main program. Then we compare the results of all the CUs, and we decided who are the new centroids.If we have convergence we stop the algorithm, otherwise we start a new iteration.
