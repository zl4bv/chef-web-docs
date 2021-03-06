.. The contents of this file are included in multiple topics.
.. This file should not be changed in a way that hinders its ability to appear in multiple documentation sets.


|bitbucket| integration with |chef delivery| allows |bitbucket| to be used as the canonical |git| repository for projects while at the same time benefiting from |chef delivery| workflow and pipeline automation. When |bitbucket| integration is enabled for a project in |bitbucket|, you will be able to:

* Review pull requests and make code comments in the |bitbucket| webui.
* Browse code (including in-flight changes in the |chef delivery| pipeline) using |bitbucket|.
* Have the target branch (usually master) of a |bitbucket| project repository be managed by |chef delivery|. When a change is approved in |chef delivery|, it will perform the merge in |bitbucket|.

|bitbucket| integration is designed for use with |bitbucket| 3.x and supports connecting a |chef delivery| enterprise with a single |bitbucket| server URL. |chef delivery| communicates with |bitbucket| using the |bitbucket| REST API and HTTP-based |git| protocol. |chef delivery| authenticates with |bitbucket| with a user account over HTTP basic authentication. It is recommended that the connection be protected using SSL.
