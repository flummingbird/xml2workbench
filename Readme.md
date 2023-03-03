# Extract, transform, load pipeline for LDL Collections

You should follow steps to download LDL metadata. There are 2 main ways to do so. One requires server access, the other uses a shell script to curl down datastreams from the LDL website.


### 1)  Extract your DATASTREAM files (rdf, xml, obj)to a directory.
####    A) Getting PidList:
      a.  USE lsu vpn
      b.  ssh to lsu subnet machine
      c.  ssh to dgi-ingest server (hosted on aws)
      d.  drush -u 1 islandora_datastream_crud_fetch_pids --collection="hnoc-aww:collection" --pid_file=/tmp/collection-data/hnoc-aww:collection.txt
      e.  drush -u 1 idcrudfd --pid_file=/tmp/collection-data/hnoc-aww\:collection.txt --datastreams_directory=/tmp/collection-data/ds --dsid=OBJ
      f.  How do we identify compounds, and get their children?
         1. Example: https://ingest.louisianadigitallibrary.org/islandora/object/hnoc-aww%3A40/manage/
         2. Search for content type, Use machine name from link{1}/collection: compoundCModel, sp_pdf, sp_large_image_cmodel

####    B) downloading the Datastream files:
    target collection ==>hnoc-awww:collection
    visit ==> https://ingest.louisianadigitallibrary.org/islandora/object/hnoc-aww:collection/manage
    PIDS written to file /tmp/hnoc-aww-namespaced.txt 

      a.  after login via server and nav to drupal root (aka /var/www/data)
      b.  drush -u 1 islandora_datastream_crud_fetch_pids --namespace=hnoc-aww --pid_file=/tmp/hnoc-aww-namespaced.txt
      c.  mkdir /tmp/hnoc
      d.  Run these drush commands in sequence:
            drush -u 1 idcrudfd --pid_file=/tmp/hnoc-aww-namespaced.txt --datastreams_directory=/tmp/hnoc --dsid=OBJ
            drush -u 1 idcrudfd --pid_file=/tmp/hnoc-aww-namespaced.txt --datastreams_directory=/tmp/hnoc --dsid=RELS-EXT
            drush -u 1 idcrudfd --pid_file=/tmp/hnoc-aww-namespaced.txt --datastreams_directory=/tmp/hnoc --dsid=MODS
      e.  compress the datastreams for export:
            cd /tmp/hnoc
            zip hnoc-data.zip *      
      f.  move the file to the /tmp directory so it is accessible
            mv hnoc-data.zip /tmp/
      g.  log out of the ingest server, back into the computer on the subnet
            type control+D to logout aka ctl-D
      h.  scp the file:
            scp dgi-ingest:/tmp/hnoc-data.zip ~/Downloads
      i.  CTL+D (to log out of subnet machine.)
      j.  copy to other computes as needed.   
            scp Work:/tmp/hnoc-data.zip ~/Downloads/  
            
### 2)  Debug and Run xml2workbench.py (metadata from xml tool initially scripted by Rosie Le Faive), the tool that extracts metadata from xml tool scripted by Rosie Le Faive:
    A) The original code was edited to fix Nan-type errors in order to have the code work on creating the right metadata fields according to LDL fields (Documentation in https://github.com/Miladkhanlou/XML-to-Metadata-Tool )
    B) Change the input directory in xml2workbench.py to the directory in which you have extracted the MODS.xml
       files for a given collection.
    C) Run the xml2workbench.py  ==>  python3 mod2workbench.py
    
### 3) Running metadata_process.py, a customized python script for processing, cleaning and fixing data in the dataset.
    - To learn more about how script works please visit: https://github.com/Miladkhanlou/Post-Processing-Tool-For-LDL.
### 4) Using Processed and cleaned metadata now every thing is ready to run Islandora workbench to ingest data to the new website 
    - See documentation on how to install and configure isladnora workbench ingestion tool and running using bash in the link bellow.
