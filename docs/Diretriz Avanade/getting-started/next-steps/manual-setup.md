# Manual Setup

BCA setups must be primarily done via the BizApps Platform. There can be exceptions where this is not possible due to client concerns or security policies. If this is explicitly confirmed by the Solution Architect, Delivery Lead and client stakeholders, the BCA support team can perform a manual setup with the following steps:

1. Create an empty project in a personal DevOps organization.
1. Set up a project through the BizApps Platform.
1. Request a DevOps project in the clients DevOps with Admin access.
1. Import the Repository from the source DevOps to the target DevOps.
1. Set up target DevOps Project:
    1. Create "Dataverse Secrets" variable group with same/similar content from the source DevOps.
    1. Add the "{ProjectName} Build Service ({OrgName})" user to the "Contributor" permission group.
    1. Create the three pipelines ("Dataverse Guard", "Dataverse Deploy" and "Dataverse Deploy Code") from existing YAML.
