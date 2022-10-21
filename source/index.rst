Head in the clouds
==============================================

Virtualisation technology in the cloud has become a huge resource to leverage, as it promises high availability
and access to resources from anywhere ... meanwhile, none of the cloud suppliers protect the data under their care.
They protect the security of the cloud, not the data in the cloud. Responsibility for the CIA (confidentiality,
integrity, and availability) of the data in the cloud, remains with the data owner. Data in the cloud is running on
someone else's server, and the data is often in the clear, in motion and in use.

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Challenges

   docs/challenges/README.md
   docs/challenges/transparency.md
   docs/challenges/sharing.md
   docs/challenges/policies.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Information gathering

   docs/recon/README.md
   docs/recon/map.md
   docs/recon/scanning.md
   docs/recon/s3-urls.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Account and privilege attacks

   docs/roles/README.md
   docs/roles/harvesting.md
   docs/roles/escalation.md
   docs/roles/takeover.md
   docs/roles/spraying.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Misconfigured cloud assets

   docs/misconfigured/README.md
   docs/misconfigured/iam.md
   docs/misconfigured/federation.md
   docs/misconfigured/object-storage.md
   docs/misconfigured/containers.md

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: Cloud-centric attacks

   docs/cloud-centric/README.md
   docs/cloud-centric/dos.md
   docs/cloud-centric/malware-injection.md
   docs/cloud-centric/ssti.md
   docs/cloud-centric/side-channel.md
   docs/cloud-centric/sdk.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: CI/CD pipelines

   docs/cicd/README.md
   docs/cicd/supply-chain.md

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Prevention

   docs/prevent/README.md
   docs/prevent/identify.md
   docs/prevent/quantify.md
   docs/prevent/prioritise.md
   docs/prevent/tools.md

.. toctree::
   :caption: Links

   Red Team <https://tymyrddin.github.io/red/>
