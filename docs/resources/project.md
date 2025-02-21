---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "wiz_project Resource - terraform-provider-wiz"
subcategory: ""
description: |-
  Projects let you group your cloud resources according to their users and/or purposes.
---

# wiz_project (Resource)

Projects let you group your cloud resources according to their users and/or purposes.

## Example Usage

```terraform
# A simple example
resource "wiz_project" "test" {
  name        = "Test App"
  description = "My project description"
  risk_profile {
    business_impact = "MBI"
  }
  business_unit = "Technology"
}

# This resource contains multiple organization links, one with tags and another without
resource "wiz_project" "test" {
  name        = "Test App"
  description = "My project description"
  risk_profile {
    business_impact = "MBI"
  }
  business_unit = "Technology"
  cloud_organization_link {
    cloud_organization = "7edbb879-9960-513f-b56d-876e9db2a962"
    environment        = "PRODUCTION"
    shared             = false
  }
  cloud_organization_link {
    cloud_organization = "07401938-0347-5a02-80eb-db19eecfbf98"
    environment        = "PRODUCTION"
    shared             = true
    resource_tags {
      key   = "application"
      value = "Wiz"
    }
    resource_tags {
      key   = "environment"
      value = "production"
    }
  }
}

# This resource contains a single cloud account link, with tag
resource "wiz_project" "test" {
  name        = "Test App"
  description = "My project description"
  risk_profile {
    business_impact = "MBI"
  }
  business_unit = "Technology"
  cloud_account_link {
    cloud_account_id = "3225def3-0e0e-5cb8-955a-3583f696f778"
    environment      = "PRODUCTION"
    resource_tags {
      key   = "created_by"
      value = "terraform"
    }
  }
}

# This resource contains a single kubernetes cluster link
resource "wiz_project" "test" {
  name        = "My Kubernetes Project"
  description = "My project description"
  risk_profile {
    business_impact = "MBI"
  }
  business_unit = "Technology"
  kubernetes_cluster_link {
    kubernetes_cluster = "77de7ca1-02f9-5ed2-a94b-5d19c683efaf"
    environment        = "STAGING"
    shared             = true
    namespaces         = ["kube-system"]
  }
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `name` (String) The project name to display in Wiz.

### Optional

- `archived` (Boolean) Whether the project is archived/inactive
    - Defaults to `false`.
- `business_unit` (String) The business unit to which the project belongs.
- `cloud_account_link` (Block Set) Associate the project directly with a cloud account by wiz identifier UID to organize all the subscription resources, issues, and findings within this project. (see [below for nested schema](#nestedblock--cloud_account_link))
- `cloud_organization_link` (Block Set) Associate the project with an organizational link to organize all the subscription resources, issues, and findings within this project. (see [below for nested schema](#nestedblock--cloud_organization_link))
- `description` (String) The project description.
- `identifiers` (List of String) Identifiers for the project.
- `kubernetes_cluster_link` (Block Set) Associate the project with kubernetes clusters. (see [below for nested schema](#nestedblock--kubernetes_cluster_link))
- `project_owners` (List of String) A list of project owner IDs.
- `risk_profile` (Block List, Max: 1) Contains risk profile related properties for the project (see [below for nested schema](#nestedblock--risk_profile))
- `security_champions` (List of String) A list of security champions IDs.

### Read-Only

- `id` (String) Unique identifier for the project.
- `slug` (String) Short identifier for the project. The value must be unique, even against archived projects, so a uuid is generated and used as the slug value.

<a id="nestedblock--cloud_account_link"></a>
### Nested Schema for `cloud_account_link`

Required:

- `cloud_account_id` (String) The Wiz internal identifier for the Cloud Account Subscription.

Optional:

- `environment` (String) The environment.
    - Allowed values: 
        - PRODUCTION
        - STAGING
        - DEVELOPMENT
        - TESTING
        - OTHER

    - Defaults to `PRODUCTION`.
- `resource_groups` (List of String) Please provide a list of resource group identifiers for filtering by resource groups. `shared` must be true to define resource_groups.
- `resource_tags` (Block Set) Provide a key and value pair for filtering resources. `shared` must be true to define resource_tags. (see [below for nested schema](#nestedblock--cloud_account_link--resource_tags))
- `shared` (Boolean) Subscriptions that host a few projects can be marked as ‘shared subscriptions’ and resources can be filtered by tags.

<a id="nestedblock--cloud_account_link--resource_tags"></a>
### Nested Schema for `cloud_account_link.resource_tags`

Required:

- `key` (String)
- `value` (String)



<a id="nestedblock--cloud_organization_link"></a>
### Nested Schema for `cloud_organization_link`

Required:

- `cloud_organization` (String) The Wiz internal identifier for the Organizational Unit.

Optional:

- `environment` (String) The environment.
    - Allowed values: 
        - PRODUCTION
        - STAGING
        - DEVELOPMENT
        - TESTING
        - OTHER

    - Defaults to `PRODUCTION`.
- `resource_groups` (List of String) Please provide a list of strings for filtering by resource groups. `shared` must be true to define resource_groups.
- `resource_tags` (Block Set) Provide a key and value pair for filtering resources. `shared` must be true to define resource_tags. (see [below for nested schema](#nestedblock--cloud_organization_link--resource_tags))
- `shared` (Boolean) Subscriptions that host a few projects can be marked as ‘shared subscriptions’ and resources can be filtered by tags.
    - Defaults to `true`.

<a id="nestedblock--cloud_organization_link--resource_tags"></a>
### Nested Schema for `cloud_organization_link.resource_tags`

Required:

- `key` (String)
- `value` (String)



<a id="nestedblock--kubernetes_cluster_link"></a>
### Nested Schema for `kubernetes_cluster_link`

Required:

- `kubernetes_cluster` (String) The Wiz internal identifier for the kubernetes cluster.

Optional:

- `environment` (String) The environment.
    - Allowed values: 
        - PRODUCTION
        - STAGING
        - DEVELOPMENT
        - TESTING
        - OTHER

    - Defaults to `PRODUCTION`.
- `namespaces` (List of String) The kubernetes namespaces to link. `shared` must be set to `true` if namespaces are set.
- `shared` (Boolean) Mark the kubernetes cluster as shared, in which case, specific namespaces can be linked. This needs to be set to `true` if `namespaces` are set.
    - Defaults to `true`.


<a id="nestedblock--risk_profile"></a>
### Nested Schema for `risk_profile`

Optional:

- `business_impact` (String) Business impact.
    - Allowed values: 
        - LBI
        - MBI
        - HBI
- `has_authentication` (String) Does the project require authentication?
    - Allowed values: 
        - YES
        - NO
        - UNKNOWN

    - Defaults to `UNKNOWN`.
- `has_exposed_api` (String) Does the project expose an API?
    - Allowed values: 
        - YES
        - NO
        - UNKNOWN

    - Defaults to `UNKNOWN`.
- `is_actively_developed` (String) Is the project under active development?
    - Allowed values: 
        - YES
        - NO
        - UNKNOWN

    - Defaults to `UNKNOWN`.
- `is_customer_facing` (String) Is the project customer facing?
    - Allowed values: 
        - YES
        - NO
        - UNKNOWN

    - Defaults to `UNKNOWN`.
- `is_internet_facing` (String) Is the project Internet facing?
    - Allowed values: 
        - YES
        - NO
        - UNKNOWN

    - Defaults to `UNKNOWN`.
- `is_regulated` (String) Is the project regulated?
    - Allowed values: 
        - YES
        - NO
        - UNKNOWN

    - Defaults to `UNKNOWN`.
- `regulatory_standards` (List of String) Regulatory Standards.
    - Allowed values: 
        - ISO_20000_1_2011
        - ISO_22301
        - ISO_27001
        - ISO_27017
        - ISO_27018
        - ISO_27701
        - ISO_9001
        - SOC
        - FEDRAMP
        - NIST_800_171
        - NIST_CSF
        - HIPPA_HITECH
        - HITRUST
        - PCI_DSS
        - SEC_17A_4
        - SEC_REGULATION_SCI
        - SOX
        - GDPR
- `sensitive_data_types` (List of String) Sensitive Data Types.
    - Allowed values: 
        - CLASSIFIED
        - HEALTH
        - PII
        - PCI
        - FINANCIAL
        - CUSTOMER
- `stores_data` (String) Does the project store data?
    - Allowed values: 
        - YES
        - NO
        - UNKNOWN

    - Defaults to `UNKNOWN`.
