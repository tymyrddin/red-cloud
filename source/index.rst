Head in the clouds
==============================================

Virtualisation technology in the cloud has become a huge resource to leverage, as it promises high availability
and access to resources from anywhere ... meanwhile, none of the cloud suppliers protect the data under their care.
They protect the security of the cloud, not the data in the cloud. Responsibility for the CIA (confidentiality,
integrity, and availability) of the data in the cloud, remains with the data owner. Data in the cloud is running on
someone else's server, and the data is often in the clear, in motion and in use.

.. image:: _static/images/in-progress.png
  :alt: Forever in progress ...

----

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Test lab

   AWS tools <https://red.tymyrddin.dev/projects/testlab/en/latest/docs/cloud/aws.html>

----

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Preparation

   Reconnaissance <https://red.tymyrddin.dev/projects/recon/en/latest/docs/cloud/README.html>
   Enumeration <https://red.tymyrddin.dev/projects/enum/en/latest/docs/system/cloud.html>

----

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

----

Attacking and defending AWS
---------------------------------------------

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: IAM

   docs/iam/README.md
   docs/iam/enum-iam.md
   docs/iam/mis-trust-policy.md
   docs/iam/overly-permissive-1.md
   docs/iam/dangerous-policy-1.md
   docs/iam/dangerous-policy-2.md
   docs/iam/overly-permissive-2.md
   docs/iam/pass-role-ec2.md
   docs/iam/pass-role-lambda.md
   docs/iam/pass-role-formation.md
   docs/iam/api-gateway-enum.md
   docs/iam/verb-tampering.md
   docs/iam/mis-private-api.md
   docs/iam/iam-auth.md
   docs/iam/dos.md
   docs/iam/poor-lambda-authoriser.md

----

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: More

   Bust-a-Kube <https://www.bustakube.com/>
   flAWS 2 <http://flaws2.cloud/>
   AWSGoat <https://github.com/ine-labs/AWSGoat>
   CloudGoat <https://github.com/RhinoSecurityLabs/cloudgoat>

----

.. image:: _static/images/books.png
  :alt: Useful books
