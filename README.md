# ModelMatch3D - A toolset for facilitating forensic individual identification based on mandibular or maxillary 3D models

`ModelMatch3D` provides fast alignment of mandibular or maxillary 3D models and calculates simple fitness metrics, allowing for quick database queries for forensic identification of post-mortem samples. Unlike other methods, it does not require the initial 3D models to be aligned or properly cleaned. It can also help identify partial samples (e.g. with missing parts). The module allows pairwise comparison of query and reference samples with visual feedback, as well as large database queries using one or multiple query samples. Keep in mind, however, that ModelMatch3D is meant as an aid to forensic identification, not as a replacement for a properly trained forensic scientist. Conclusions about forensic identification shold be performed by a professional.

## Tutorial

Open the `ModelMatch3D` module. It is located under `Forensic Odontology` in 3D Slicer's dropdown menu.

When invoked for the first time, `ModelMatch3D` will need your permission to download the `open3D` library. Depending on the internet speed, download may take a few minutes but this is a one-time event. You may need to restart Slicer after installation.

Once everything is installed, you should observe the following screen:

<p align="center">
<img src="./images/ModelMatch3D001.png"> 
</p>


There are three main tabs in `ModelMatch3D` : a `Pair Matching`, a `Database processing`, and an `Advanced Settings` one.

* __Pair Matching tab__: this is the default tab when opening ModelMatch3D. It is used to perform pairwise comparisons of query and reference samples with quick visual feedback. It is most useful when needing to visually check for potential matches, as well as creating simple heatmaps for visualizing differences between query and reference models. It also displays color-coded indexes of intersample matching, indicating the degree to which ModelMatch3D considers query and reference samples as likely to be from the same individual : 1) likely (green), 2) uncertain (yellow), 3) unlikely (red).

<p align="center">
<img src="./images/ModelMatch3D004.png">
</p>


* __Database processing__: this tab allows for quick database searches using one or multiple query models against a reference database. It is useful for querying large databases fast and efficiently. It displays promising pairwise matching sample IDs as rows in a table. It also indicates the extent to which the query has been completed using a simple progress bar.

<p align="center">
<img src="./images/ModelMatch3D002.png">
</p>

* __Advanced Settings__: this tab allows users to change settings for ModelMatch3D. This is generally not recommended for novice users. Therefore, we can keep the default settings for this tutorial. For more details, please keep an eye on an upcoming publication.

<p align="center">
<img src="./images/ModelMatch3D003.png">
</p>


## Pair Matching

Now that we are acquainted with the overall layout of the module, let's start by aligning a single query model to a reference one.

### Step 1. Specifying the query and reference models. 
From a local repository, drag-and-drop your query and reference models into Slicer. Currently, ModelMatch3D can handle most widely used model formats, such as `.ply`, `.stl`, `.obj` and `.vtk` formats.

You can also use Slicer's default data loading procedure by clicking the `Data load` button, then navigating to your working directory, and selecting both models.

 * If everything worked properly, you should observe something similar to:

<p align="center">
<img src="./images/ModelMatch3D005.png">
</p>


* As can be seen above, the two models lie in arbitrary positions in 3D space and are not properly cleaned. Contrary to other approaches, `ModelMatch3D` can generally deal with arbitrary positioned models and/or models that are not properly segmented.

* Return to the `ModelMatch3D` module and select the query model as well as the reference model using the dropdown menus under the `Pair Matching` tab: 

  * __Query Model__: Select the model whose identification is unknown. All statistics generated by ModelMatch3D are relative to the query model. As such, it is generally ok to use mandibular and maxillary fragments as queries. Similarly, it is generally ok to use models that have not been perfectly segmented as queries. However, ModelMatch3D 's performance should increase when using complete samples that have been properly segmented.

  * __Reference Model__: Under the `Reference Model`, the user is expected to select a model whose identification is known. Ideally, the user should have a prior hypothesis that the reference model represents the same individual as the query model. Generally, reference models should represent samples that are as complete as possible. Similar to the query model, reference models can be used even if not perfectly segmented.


### Step 2. Run ModelMatch3D.
Once query and reference models are selected, we can start the ModelMatch3D pipeline by pressing the `Run` button. This step will execute the whole intersample alignment:

* Downsampling query and reference models based on the `Point Density Ajustment` value (Advanced Settings). 

* Global (RANSAC) and rigid (ICP) registration that register the query model relative to the reference one

* Calculation of pairwise fitness metrics that take into account all points (Total) or a subset of points (Partial) in the query model.

* Display of the fitness metrics (Matching evaluation) using color-coded bars for quick visual feedback. The colors follow the current scheme: (1) likely match: green bar; (2) uncertain match: yellow; (3) unlikely match: red. 

This color scheme was defined based on large-scale comparisons of 3D models, but might not apply in all cases. So, they are not to be taken at face value. In particular, large remodeling during an individual's lifetime can lead to poor matching evaluation of samples belonging to the same individual.

<p align="center">
<img src="./images/ModelMatch3D007.png">
</p>


### Step 3. Display options.
By default, after pressing the `Run` button, both the aligned reference and query models are displayed to the user. In this section, the user has the option to hide one or more of the models using switch buttons, as well as generating a heatmap of differences between the query and reference samples overlayed upon the original query model.


<p align="center">
<img src="images/ModelMatch3D008.png">


 
* The output of each pairwise ModelMatch3D run is not saved to disk. The Pair Matching tab is purely for visual inspection/feedback. For a complete database search that saves the ouput to disk, please use the `Database processing` tab. 


<p align="center">
<img src="images/ModelMatch3D009.png">



### Step 4 (optional and not recommended). Alter the ModelMatch3D settings and run another comparison. 
 
In general, we do not recommend changing the ModelMatch3D settings. The default settings have achieved good performance across multiple datasets and changing them is unlikely to improve the performance of ModelMatch3D.

However, if one so wishes, several parameters can be adjusted: 

    * Increasing point density, which can lead to smaller voxel size, and possible improvement of the registration.

    * Increasing `maximum RANSAC iterations` and `RANSAC confidence` in the `Rigid registration` section. 

    * Other parameters in the `Rigid registration` section can also be fine tuned. 
  

## Database processing 

As mentioned in the prior section, the main purpose of the `Pair Matching` tab is to provide visual feedback to a suspected query-reference match. In `ModelMatch3D`, the parameters used in the `Pair Matching` tab get transferred to the `Database processing` tab once that tab gets selected. 

* In the `Database processing` tab, click the drop-down menu of `Method`. You can see two options: the default option is `Single-Query`; another option is `Muli-Query`. We will work with the default option `Single Query`. The multi-query option will follow similar steps. 
 
* Note that the `Database processing` tab has much of the same elements as the `Pair Matching` one. The main difference is that the query model is compared to an entire folder of reference models (database). 

* Select any unknown model as the `Query model`. Select a local directory containing models of individuals with known identity as the `Database models folder`.

* Choose a local ` Analysis output folder`. After this step, the `Run` button can be pressed.

<p align="center">
 <img src="images/ModelMatch3D010.png">
</p>
 


## Other Notes

`ModelMatch3D` is generally robust to situations in which a substantial amount of the original sample is missing. As such, it can align partial post-mortem samples, as long as the remaining parts are sufficient for proper alignment. See below for an example:


<p align="center">
 <img src="images/ModelMatch3D011.png">
</p>
