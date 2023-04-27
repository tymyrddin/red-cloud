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

----

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Attacking and defending AWS

   docs/aws/README.md
   docs/aws/enum-iam.md
   docs/aws/mis-trust-policy.md
   docs/aws/overly-permissive-1.md
   docs/aws/dangerous-policy-1.md
   docs/aws/dangerous-policy-2.md
   docs/aws/overly-permissive-2.md
   docs/aws/pass-role-ec2.md
   docs/aws/pass-role-lambda.md
   docs/aws/pass-role-formation.md
   docs/aws/api-gateway-enum.md
   docs/aws/verb-tampering.md
   docs/aws/mis-private-api.md
   docs/aws/iam-auth.md
   docs/aws/dos.md
   docs/aws/poor-lambda-authoriser.md

----

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: More

   docs/more/bust-a-kube.md
   docs/more/flaws2.md

----

.. image:: _static/images/books.png
  :alt: Useful books
