---

title: 'Improving Surgery and Radiotherapy with Real-time Analysis'

---

# Improving Surgery and Radiotherapy with Real-time Analysis

<span style="font-style: italic;text-align: center;display: block">December 27, 2015 — 11 mins read</span>

## Static prescan images are a poor guide for treatment

The standard procedure for both radiotherapy and surgery involves making scans and planning treatment prior to the operation. In the mean time, organs can shift, the patient breaths, and occasionally the alignment during the scan and operation do not match completely. For many situations these shifts are not important, but around sensitive organs and small tumors these can mean the difference between recovery and relapse.

## Real-time Imaging and Segmentation

The latest surgical suites and MRI technologies have made subsecond measurements a reality. These measurements are already being used to assist therapy, but the value is limited without the ability to process the images in real-time.

<span class="code">SELECT Image as ChestCT FROM PatientImages WHERE Modality="CT" AND Region="Chest"</span>
<span class="centered"><img src="images/mammo-001.gif" style="width:180px"/></span>

### Perform Segmentation

<span class="code">SELECT CHEST_SEGMENTATION(Image) as ChestSeg FROM ChestCT</span>
<span class="centered"><img src="images/mammo-002.gif" style="width:180px"/></span>

### Extract the heart since it is the most dose-sensitive

<span class="code">SELECT HeartRegion FROM ChestSeq</span>

<span class="centered"><img src="images/mammo-003.gif" style="width:180px"/></span>

### Track the Position of the Heart

<span class="code">SELECT CenterOfMass(roi) FROM HeartRegion GROUP BY time</span>

<span class="centered"><img src="images/mammo-004.png" style="width:100%"/></span>

It is also possible to create outlines from the structures which can then be used to update surgical plans, and provide information to the latest generation of devices which can then updating the treatment plan in real time.

<span class="code">SELECT CreateOutlines(roi) FROM HeartRegion GROUP BY Organ</span>

<span class="centered"><img src="images/mammo-005.png" style="width:100%"/></span>

### *Image Query and Analysis Engine*

Despite recent boosts in popularity, both personalized and precision medicine are typically far removed from the clinical setting. In many of these cases, all of the data required for significantly better informed diagnosis. Hospitals are required to archive a majority of patient imaging studies and scans for a minimum of 10 years. These essential data are just sitting unused on PACS, tapes, and other storage volumes

*IQAE* is the key to unlocking the potential of this data and turning it from a burden into an asset. The data can be analyzed in a completely anonymized manner to obtain general, precise statistics as references against which to compare new patients. Additionally careful analysis of all data can reveal mistakes, missed diagnoses, and new potential high risk patients.

### Machine Learning

The quantitatively meaningful data can then be used to train machine learning algorithms (decision trees to SVM) in order to automatically high-light many of the interesting regions and label them as such, dratistically reducing the required time to analyze a single patient.

Here we show a simple decision tree trained to identify lesions using color, position, texture and shape.

<span class="centered"><img src="images/mammo-006.png" style="width:100%"/></span>

Furthermore the ability to parallelize and scale means thousands to millions of videos can be analyzed at the same time to learn even more about the structures of the digestive track and identify new possibilities for diagnosis.

### How

The first question is how the data can be processed. The basic work is done by a simple workflow on top of our Spark Image Layer. This abstracts away the complexities of cloud computing and distributed analysis. You focus only on the core task of image processing.

<span class="centered"><img src="images/mammo-007.svg" style="width:200px"/></span>

The true value of such a scalable system is not in the single analysis, but in the ability to analyze hundreds, thousands, and even millions of samples at the same time.

<span class="centered"><img src="images/mammo-008.svg" style="width:100%"/></span>

With cloud-integration and Big Data-based frameworks, even handling an entire city network with 100s of drones and cameras running continuously is an easy task without worrying about networks, topology, or fault-tolerance.

## Technical Aspects

### Processing the Data

Once the cluster has been comissioned and you have the StreamingSparkContext called <code>ssc</code> (automatically provided in [Databricks Cloud or Zeppelin](https://databricks.com/product/databricks)), the data can be loaded using the Spark Image Layer. Since we are using real-time analysis, we acquire the images from a streaming source.

<span class="code">
  val mriScanner = MedicalCameraReceiver("https://mri-scanner-8091")<br/>
  val metaImageStream = ssc.receiverStream(mriScanner)
</span>

Although we execute the command on one machine, the analysis will be distributed over the entire set of cluster resources available to ssc. To further process the images, we can take advantage of the rich set of functionality built into Spark Image Layer. The entire pipeline can then be started to run in real-time on all the new images as they stream in. If the tasks become more computationally intensive, then the computing power can be scaled up and down elastically using on-premise or cloud-based machines based on the regulatory nature of the environment.

## Learn More

*4Quant* is active in a number of different areas from medicine to remote sensing. Our image processing framework (Spark Image Layer) and our query engine (*Image Query and Analysis Engine*) are widely adaptable to a number of different specific applications.

Check out our other use-cases to see how *4Quant* can help you

### Medicine

<div class="news">
  <div>December 27 2015</div>
  <p>
    <a href="#">Planning surgery and radiotherapy with real time segmentations</a>
  </p>
  <div>December 27 2015</div>
  <p>
    <a href="#">Segmenting organs from archived chest CT images</a>
  </p>
  <div>December 27 2015</div>
  <p>
    <a href="#">Finding lesions in Capsule Based Endoscopy</a>
  </p>
</div>

### Geographic Information Systems

<div class="news">
  <div>December 27 2015</div>
  <p>
    <a href="#">Calculating Flood Risk for Insurance Companies</a>
  </p>
  <div>December 27 2015</div>
  <p>
    <a href="#">Counting Cars in Satellite Images</a>
  </p>
  <div>December 27 2015</div>
  <p>
    <a href="#">Finding buildings and forests in Satellite Images</a>
  </p>
</div>

### Surveillance

<div class="news">
  <div>December 27 2015</div>
  <p>
    <a href="#">Count people from drone footage</a>
  </p>
  <div>December 27 2015</div>
  <p>
    <a href="#">Finding criminals with traffic cameras</a>
  </p>
</div>

### Real-time QA

<div class="news">
  <div>December 27 2015</div>
  <p>
    <a href="#">Check train tracks in real time</a>
  </p>
</div>

### Fun

<div class="news">
  <div>December 27 2015</div>
  <p>
    <a href="#">Untangling the flood of Online Dating</a>
  </p>
  <div>December 27 2015</div>
  <p>
    <a href="#">Quantitative Image Search Machine</a>
  </p>
</div>

## Technical Presentations

To find out more about the technical aspects of our solution, check out our presentation at:

<div class="news">
  <div>December 27 2015</div>
  <p>
    <a href="#">Spark Summit or watch the video.</a>
  </p>
  <div>December 27 2015</div>
  <p>
    <a href="#">Synchrotron Radiation Instrumentation 2015</a>
  </p>
  <div>December 27 2015</div>
  <p>
    <a href="#">ICTMS 2015</a>
  </p>
  <div>December 27 2015</div>
  <p>
    <a href="#">LifeScienceForumBasel 2015</a>
  </p>
</div>
