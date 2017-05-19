.. _developmentguide-label:

Development Guidelines
======================

Merging pull requests
---------------------
For code changes, we currently use a guideline of `lazy consensus <http://www.apache.org/foundation/glossary.html#LazyConsensus/>`_. with two positive reviews with at least one of those reviews being one of the core maintainers and no negative votes. And of course, the gates for the pull requests must pass as well (unit tests, etc).

We use the +1/-1 reactions as part of the review. Once there are enough, a core committer reviews and approves the pull request itself.

Getting commit privileges
-------------------------
The core committer team uses a `lazy consensus <http://www.apache.org/foundation/glossary.html#LazyConsensus/>`_ mechanism to grant contributor rights to Project Symphony. Any maintainer/core contributor can nominate someone to have those privileges. The team grants commit privileges with two +1 votes and no negative votes.

The core team is also responsible for removing commit privileges when appropriate - for example, for malicious merge behavior or just inactivity over an extended period of time.

Quality gates for the pull requests
-----------------------------------
Currently the only quality gate to ensure pull request quality is located on Jenkins. All information related to stages is in the JenkinsFile. Additional quality gates are planned.

Naming conventions
------------------

+-------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|Naming Convention  | Description                                                                                                                                                            |
+===================+========================================================================================================================================================================+
| Maven             |  If you create a new feature that creates a new JAR file, add the following groupID in Maven for consistency: ``<groupId>com.dell.cpsd</groupId>``                     |
+-------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Docker            |  Use the following convention to name all docker images: ``[cpsd]-[function]-[service]``                                                                               |
+-------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Version conventions
-------------------


The version label contains four numbers, based on `Semantic Versioning
<http://semver.org/>`_.

``<major>.<minor>.<patch>.<customer>``

Version numbers can be incremented differently depending on the project requirements.

* Increment the major version for backwards-incompatible changes.

* When adding entire new features, you might increment the minor version.

* For backwards-compatible changes (for example, bug fixes), increment the patch identifier.

* The fourth number represents a customer patch that cannot be properly made with the patch version ID.

Code versioning
~~~~~~~~~~~~~~~
Code versioning (for example, JAR files) follows fairly standard open source versioning guidelines. When developing for a new release, use a code versioning format that signifies the code is under active development. As the code is being released, update the version to remove the signifier that the code is under development.
hen the next development period starts, increment the version and add the marker to signify the code is under active development.        For Java development, use the following convention:   

* Active Development: ``<version>-SNAPSHOT``                                                                                              
* Released: ``<version>``   
 
Docker container versioning
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Docker container version appears in both the image tag and as labels within the Docker image. 

Docker image tag
++++++++++++++++

A Docker image has a tag that indicates the service name and version. An additional Docker image tag (think symlink or alias) shows the service version and build number. This additional tag allows you to quickly see from which build a given image comes without needing to know more in-depth Docker commands.

* The image uses a tag based on the image name and the release version: ``hello-world:<version>``

* An additional tag is created that also includes the build number: ``hello-world:<version>-b<build number>``

The Docker compose file for a PAQX uses the image with just the name and version. This ensures the Docker compose file is not updated for each build of a given version.

Docker image label
++++++++++++++++++

Include a label for the release version: ``LABEL com.dell.cpsd.version:<version>``

Include a label for the build number: ``LABEL com.dell.cpsd.build_number:<build number>``

Include a label for the code repository version: ``LABEL com.dell.cpsd.git_revision:<git repo version>``

Deployment versioning (RPM)
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Deployment files are versioned to signify active development and released versions. To help with traceability, both versioning types include an additional value that is the continuous integration (CI) (for example, Jenkins) build number for the project. Development versions include an additional identifier that specifies the version is under development and should not be released to consumers.

* Active development: ``<version>-<CI build number>-<date>git`` (https://fedoraproject.org/wiki/Packaging:Versioning#Snapshot_packages)

* Released: ``<version>-<CI build number>``
