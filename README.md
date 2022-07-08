# IBM Business Automation Manager Open Editions 8 OpenShift images

This repository contains all the image descriptors and files necessary to build the IBM BAMOE images.


### Repo structure:

- **rhpam-7-openshift-image**
  - [businesscentral](businesscentral): Business Central Container Image descriptor files.
  - [businesscentral-monitoring](businesscentral-monitoring): Business Central Monitoring Container Image descriptor files.
  - [controller](controller): IBM BAMOE Controller Container Image descriptor files.
  - [dashbuilder](dashbuilder): IBM BAMOE Dashbuilder Container Image descriptor files.
  - [kieserver](kieserver): KIE Execution Server Container Image descriptor files.
  - [process-migration](process-migration): Process Instance Migration Container Image descriptor files.
  - [quickstarts](quickstarts): Quickstarts used as example on IBM BAMOE images.
  - [scripts](scripts): Holds scripts to help this repository maintenance.
  - [smartrouter](smartrouter): IBM BAMOE Smart Router image descriptor files.
  - [templates](templates): Templates files to build your own JDBC extension driver images using the JBoss KIE JDBC Driver Extension Images
    - [contrib](templates/contrib)
      - [jdbc](templates/contrib/jdbc): Examples about how to add third party jdbc driver on IBM BAMOE images.

Inside each image directory you will find the following files:

 - **container.yaml**: used by OSBS builds.
 - **content_sets.yaml**: define the yum repositories needed to install dependencies for the image.
 - **branch-overrides.yaml**: overrides file which uses the latest stable version for external dependencies.
 - **tag-overrides.yaml**: Used to override the branchs to use the final tags to rebuild released images, for CVE respins.
 - **image.yaml**: the main image descriptor file, here are all the pieces and configuration needed to build an image.


On the root directory you can also find one additional file:

 - [ibm-bamoe-image-streams.yaml](ibm-bamoe8-image-streams.yaml): imagestreams definitions, file used to install the product image streams on OpenShift, this files contains the image stream name and the registry the image will be pulled of.


This repo depends directly on 5 repositories, which are:

 - [cct_module](https://github.com/jboss-openshift/cct_module.git): contains some core modules, like s2i and maven.
 - [jboss-eap-modules](https://github.com/jboss-container-images/jboss-eap-modules.git): contains modules responsible to configure JBoss EAP, like datasources and the base configuration files.
 - [jboss-eap-image](https://github.com/jboss-container-images/jboss-eap-7-image.git): provides the EAP modules, which is used to install the EAP on the target image.]
 - [wildfly-cekit-modules](https://github.com/wildfly/wildfly-cekit-modules.git): provides the EAP configuration modules, used to configure the EAP during startup.
 - [rhpam-7-image](https://github.com/jboss-container-images/rhpam-7-image.git): provides the IBM BAMOE binaries modules.
 - [jboss-kie-modules](https://github.com/jboss-container-images/jboss-kie-modules): contains all the resources used to configure the IBM BAMOE images.


#### Building Images

Notice that, [CEKit](https://cekit.io/) 3.8 or higher is required to build IBM BAMOE images.

To build the images for local testing there is a [Makefile](./Makefile) which will do all the hard work for you.
With this Makefile you can:

- Build and test all images with only one command:

     ```bash
     make
     ```
     If there's no need to run the tests just set the *ignore_test* env to true, e.g.:

     ```bash
     make ignore_test=true
     ```

- Test all images with only one command, no build triggered, set the *ignore_build* env to true, e.g.:

     ```bash
     make ignore_build=true
     ```

- Build images individually, by default it will build and test each image

     ```bash
     make image image_name=businesscentral
     make image image_name=businesscentral-monitoring
     make image image_name=controller
     make image image_name=dashbuilder
     make image image_name=kieserver
     make image image_name=process-migration
     make image image_name=smartrouter
     ```
  
     We can ignore the build or the tests while interacting with a specific image as well, to build only:

     ```bash
     make ignore_test=true image_name={image_name}
     ```

     Or to test only:

     ```bash
     make ignore_build=true image_name={image_name}
     ```

- List all available Images:
    To list all available images in the repository do:

    ```bash
    make list-images
    ```

- Building the images with a different overrides file:
    This can be done by simpling provide the overrides file name that should reside on the same directory than 
    the target image.
    ```bash
    make OVERRIDES=my-overrides-file-name.yaml
    ```

##### Found an issue?

Please submit an issue [here](https://issues.jboss.org/projects/RHPAM) with the **Cloud** tag or 
send us a email: bsig-cloud@redhat.com.
