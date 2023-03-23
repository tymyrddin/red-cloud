Head in the clouds
==============================================

Virtualisation technology in the cloud has become a huge resource to leverage, as it promises high availability
and access to resources from anywhere ... meanwhile, none of the cloud suppliers protect the data under their care.
They protect the security of the cloud, not the data in the cloud. Responsibility for the CIA (confidentiality,
integrity, and availability) of the data in the cloud, remains with the data owner. Data in the cloud is running on
someone else's server, and the data is often in the clear, in motion and in use.

----

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Preparation

   Build a local testlab <https://red.tymyrddin.dev/projects/testlab/en/latest/docs/cloud/README.html>
   Reconnaissance <https://red.tymyrddin.dev/projects/recon/en/latest/docs/cloud/README.html>
   Enumeration <https://red.tymyrddin.dev/projects/enum/en/latest/docs/system/cloud.html>

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Notes on techniques

   docs/notes/README.md
   docs/notes/challenges.md
   docs/notes/accounts.md
   docs/notes/cloud-centric.md
   docs/notes/misconfigurations.md
   docs/notes/cicd.md

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: CTFs and challenges

   docs/ctf/README.md
   docs/ctf/bust-a-kube.md
   docs/ctf/flaws2.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Shift left

   docs/prevent/README.md
   docs/prevent/identify.md
   docs/prevent/quantify.md
   docs/prevent/prioritise.md
   docs/prevent/tools.md
