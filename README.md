Download Link: https://assignmentchef.com/product/solved-csye7245-assignment2-sentiment-analysis-micro-service
<br>
In this assignment, you will build a sentiment analysis micro service that could take a new Edgar file in json format and generate sentiments for each statement in the referenced EDGAR file.

To build this service, you need a Sentiment analysis model that has been trained on “labeled”, “Edgar” datasets. Note that you need to have labeled data which means someone has to label the statements and you need to use EDGAR datasets since you want the ML service to be optimized for domain-specific datasets.In order to accomplish this, we need to design 3 pipelines. ​<strong>You will have to use one of the (Dask, Luigi, Airflow, Sklearn) pipelining tools to define your pipelines.</strong>

<strong>Part 1: Annotation pipeline(Choose Dask/Airflow/Luigi/Sklearn) </strong>

The annotation pipeline will ingest a minimum of 50 earning call files from various companies and label them.

<strong>Example: </strong>

<table width="624">

 <tbody>

  <tr>

   <td width="208"><strong>Company  </strong></td>

   <td width="208"><strong>Year </strong></td>

   <td width="208"><strong>Filing </strong></td>

  </tr>

  <tr>

   <td width="208"><strong>1018724 </strong></td>

   <td width="208"><strong>2013 </strong></td>

   <td width="208"><strong>425 </strong></td>

  </tr>

 </tbody>

</table>

<ol>

 <li>Ingestion: You can use the data provided to you to ingest files and stage them in a Google/S3 bucket ; You are free to use Seeking Alpha to ingest the earnings call transcripts if you like for new data!</li>

 <li>Pre-process each file to remove white spaces and special characters. Each earnings call file is now a list of sentences.</li>

 <li>You will now have to label these lines with sentiment scores. Use text analytics APIs from these services. ​Use the api assigned to your team number:

  <ol>

   <li>Amazon Comprehend</li>

  </ol></li>

</ol>

<a href="https://docs.aws.amazon.com/comprehend/latest/dg/get-started-api-sentiment.html#get-started-api-sentiment-python">(</a>​<a href="https://docs.aws.amazon.com/comprehend/latest/dg/get-started-api-sentiment.html#get-started-api-sentiment-python">https://docs.aws.amazon.com/comprehend/latest/dg/get-started-api-sentiment.html# </a><a href="https://docs.aws.amazon.com/comprehend/latest/dg/get-started-api-sentiment.html#get-started-api-sentiment-python">get-started-api-sentiment-python</a>)​

<ol start="2">

 <li>IBM Watson</li>

</ol>

<a href="https://cloud.ibm.com/apidocs/natural-language-understanding/natural-language-understanding">(</a>​<a href="https://cloud.ibm.com/apidocs/natural-language-understanding/natural-language-understanding">https://cloud.ibm.com/apidocs/natural-language-understanding/natural-language-und </a><a href="https://cloud.ibm.com/apidocs/natural-language-understanding/natural-language-understanding">erstanding</a>)​

<ol start="3">

 <li>Google <a href="https://cloud.google.com/natural-language/docs/sentiment-tutorial">(</a>​<a href="https://cloud.google.com/natural-language/docs/sentiment-tutorial">https://cloud.google.com/natural-language/docs/sentiment-tutorial</a><a href="https://cloud.google.com/natural-language/docs/sentiment-tutorial">)</a>​</li>

 <li>Microsoft</li>

</ol>

<a href="https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis?tabs=version-3">(</a><u>​</u><a href="https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis?tabs=version-3">https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/how-tos/text</a><a href="https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis?tabs=version-3">analytics-how-to-sentiment-analysis?tabs=version-3</a><u>​</u><a href="https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis?tabs=version-3">)</a>

<ol start="4">

 <li>You will then normalize the scores to a scale of -1-1, -1 negative , 1 positive</li>

 <li>The output is a labeled dataset in csv format which would look like this. ​(Note: You will have one column not four)</li>

</ol>

<strong> </strong>

<table width="620">

 <tbody>

  <tr>

   <td width="124"><strong>Statements </strong></td>

   <td width="124"><strong>Amazon </strong></td>

   <td width="124"><strong>Google </strong></td>

   <td width="124"><strong>IBM </strong></td>

   <td width="124"><strong>Microsoft </strong></td>

  </tr>

  <tr>

   <td width="124"><strong>The quarter was terrible. We lost a lot of </strong><strong>money </strong></td>

   <td width="124"><strong>-0.8 </strong></td>

   <td width="124"><strong>-0.8 </strong></td>

   <td width="124"><strong>-0.7 </strong></td>

   <td width="124"><strong>-0.9 </strong></td>

  </tr>

  <tr>

   <td width="124"><strong>We had a wonderful quarter! </strong></td>

   <td width="124"><strong>0.9 </strong></td>

   <td width="124"><strong>0.8 </strong></td>

   <td width="124"><strong>0.8 </strong></td>

   <td width="124"><strong>0.7 </strong></td>

  </tr>

 </tbody>

</table>

<strong>Part 2: Training Pipeline(Choose Dask/Airflow/Luigi/Sklearn) </strong>

<strong>Now that we have labeled datasets, we will have to build models to train a model that will generate sentiment scores. </strong>

<ol start="5">

 <li>Model : Use the Tensorflow approach discussed in class and in [2] to use transfer learning and on the labeled dataset you created instead of the IMDB dataset to fine tune the model. 6. Save the model on to a Bucket.</li>

</ol>




<ol start="7">

 <li>Your pipeline should be configurable so that it will generate the Model based on inputs and save the trained model to a Bucket.</li>

</ol>

<strong>Part 3: Model Serving microservice: </strong>

We are now ready to deploy the model.

<ol>

 <li>You will design one microservice using Flask and the associated docker containers both exposing an API to take each sentence as the json Input and return the sentiment for each sentence.</li>

 <li>Develop a Docker container to serve an API that will take a JSON of a sentence and return back the predictions as detailed in [2]</li>

 <li>Write unit tests to test your api (See <a href="https://github.com/Harvard-IACS/2020-ComputeFest/blob/master/notebook_to_cloud/ml_deploy_demo/Makefile">https://github.com/Harvard-IACS/2020-ComputeFest/blob/master/notebook_to_cloud/ml_d </a><a href="https://github.com/Harvard-IACS/2020-ComputeFest/blob/master/notebook_to_cloud/ml_deploy_demo/Makefile">eploy_demo/Makefile</a><u>​</u><a href="https://github.com/Harvard-IACS/2020-ComputeFest/blob/master/notebook_to_cloud/ml_deploy_demo/Makefile">)</a></li>

</ol>

<strong>Input: </strong>

{“data”: [“this is the best!”, “this is the worst!”]} <strong>Output:</strong>

<strong>                 </strong>

<strong>Part 4: Inference Pipeline (Choose Dask/Airflow/Luigi/Sklearn): </strong>

<ol>

 <li>Build an inference pipeline that takes a csv file with the input in the format below</li>

</ol>

<strong>Input </strong>

<table width="624">

 <tbody>

  <tr>

   <td width="208"><strong>Company  </strong></td>

   <td width="208"><strong>Year </strong></td>

   <td width="208"><strong>Filing </strong></td>

  </tr>

  <tr>

   <td width="208"><strong>1018724 </strong></td>

   <td width="208"><strong>2013 </strong></td>

   <td width="208"><strong>425 </strong></td>

  </tr>

 </tbody>

</table>

<ol start="2">

 <li>The pipeline should dynamically get the file from EDGAR, pre-process the file,create a list of sentences for inference. Use the code you developed for the training pipeline for this.</li>

 <li>Jsonify the sentences to look something like this and invoke the <span style="text-decoration: line-through;">​2​</span> service​<span style="text-decoration: line-through;">s​</span> you created to get back the sentiments</li>

</ol>

Example inputs: {“data”: [“this is the best!”, “this is the worst!”]}

<ol start="4">

 <li>Format the output to a csv file and store it to a bucket.</li>

</ol>

<table width="624">

 <tbody>

  <tr>

   <td width="208"><strong>Statements </strong></td>

   <td width="208"><strong>Sentiment (Model 1) </strong></td>

   <td width="208"><strong><span style="text-decoration: line-through;">Sentiment (Model 2)</span> </strong></td>

  </tr>

  <tr>

   <td width="208"><strong>The quarter was terrible. We lost a lot of money </strong></td>

   <td width="208"><strong>Negative </strong></td>

   <td width="208"><strong><span style="text-decoration: line-through;">Negative</span> </strong></td>

  </tr>

  <tr>

   <td width="208"><strong>We had a wonderful quarter! </strong></td>

   <td width="208"><strong>Positive </strong></td>

   <td width="208"><strong><span style="text-decoration: line-through;">Positive</span> </strong></td>

  </tr>

 </tbody>

</table>

<strong>References: </strong>

<ol>

 <li><a href="https://github.com/microsoft/nlp-recipes/blob/master/examples/sentiment_analysis/absa/absa.ipynb">https://github.com/microsoft/nlp-recipes/blob/master/examples/sentiment_analysis/absa/ </a><a href="https://github.com/microsoft/nlp-recipes/blob/master/examples/sentiment_analysis/absa/absa.ipynb">ipynb</a></li>

 <li><a href="https://github.com/Harvard-IACS/2020-ComputeFest/tree/master/notebook_to_cloud">https://github.com/Harvard-IACS/2020-ComputeFest/tree/master/notebook_to_cloud</a></li>

 <li><a href="https://www.tensorflow.org/tutorials/keras/text_classification_with_hub">https://www.tensorflow.org/tutorials/keras/text_classification_with_hub</a></li>

</ol>