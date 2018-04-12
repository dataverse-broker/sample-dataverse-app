# os-sample-pyspark
A quick Apache Spark application for OpenShift in Python that uses [dataverse-broker](https://github.com/SamiSousa/dataverse-broker) services.

This readme is based off of [radanalytics.io](https://radanalytics.io)'s [tutorial-sparkpi-python-flask](https://github.com/radanalyticsio/tutorial-sparkpi-python-flask) project

It is intended to be
used as a source-to-image application.

## Quick start

You should have access to an OpenShift cluster and be logged in with the
`oc` command line tool.

1. Select a Dataverse service from OpenShift's Service Catalog, and create a secret. If dataverse services aren't listed, contact your cluster admin. Or, if deploying to a local cluster, follow the instructions at the (dataverse-broker)[https://github.com/SamiSousa/dataverse-broker] project.

2. Create the necessary infrastructure objects
   ```bash
   oc create -f https://radanalytics.io/resources.yaml
   ```

3. Launch spark app
   ```bash
   oc new-app --template oshinko-python-spark-build-dc  \
       -p APPLICATION_NAME=pyspark \
       -p GIT_URI=https://github.com/SamiSousa/os-sample-pyspark
   ```

4. Add the secret created in step 1 to the pyspark application.

5. Expose an external route
   ```bash
   oc expose svc/pyspark
   ```

6. Visit the exposed URL with your browser or other HTTP tool, and be prompted by the web UI.

## Explanation of certain items

The `requirements.txt` file contains requirements which setups this web application to listen on port 8080 and run the `Spark` job, as well as stating certain depencies that our scripts utilize.

The `.s2i` folder contains the environment variables for our main script which is added to the source image that is generated by radanalytics.io / Oshinko

The `dataverse_lib.py` script contains the functions for interacting with the `Dataverse API`, such as searching and downloading certain files within a dataverse, or a dataverse subtree.

The `spark_wordcount.py` script contains the logic for running the `Spark` word count file on a txt file.

The `main.py` script contains a simple `Flask` implementation which displays the word count on a browser via localhost.


