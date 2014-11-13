migx-db-to-cmp
==============

**How to create a MODX Custom Manager Page with MIGX DB**

This example is about building a Real Estate Map with Category Filter and Polygons so it covers a few extra features.

We assume you have already Installed MIGX via Package Manager

-----

##1. Create a holding Template Variable
 - define your base name, this example is "interactivemap"
 - TV name = **base_name**
 - TV Input Type = migxdb
 - Configuration = **base_name**

![TV Setup](https://dl.dropboxusercontent.com/u/4277345/MODX/migx-to-cmp/tv-setup.png)

##2. XML Schema
 - Create your XML schema, its a basis for inputs to the Database. 
 - Use the example in this repo and strip out things you don't need

##3. MIGX Package Manager Tab
 - Package name = **base_name**
 - CLICK "Create Package" Button
 - CLICK "XML Schema" Tab
 - PASTE your XML Schema
 - CLICK "Save Schema" Button
 - CLICK "Parse Schema" Tab > "Parse Schema" Button
 - CLICK "Create Tables" Tab > "Create Tables" Button
 
##4. MIGX Tab
 - name - **base_name**
 
   - **Formtabs**
   - Add 1 Item - I call mine "Published Items"
     - Inside that Add 1 item for each field in your schema file
     - typically just *fieldname* and *caption* is needed
     - you can also define your **Input TV Type** here (image | listbox | richtext | textarea | etc) 
     - listbox example: click Input Options Tab > Input Option Values > `Commercial==commercial||Single Family==single-family||Multi-Family==multi-family||Legacy==legacy`
