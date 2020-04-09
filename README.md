[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![MIT License][license-shield]][license-url]

<!-- PROJECT LOGO -->
<br />
<p align="center">
  <a href="https://github.com/dduzgun-security/github-enterprise-cloud-configuration">
    <img src="https://lp-cdn.lastpass.com/lporcamedia/-/apps/g/github-enterprise.png" alt="Logo" >
  </a>

  <h3 align="center">Github Enterprise Cloud Configuration</h3>

  <p align="center">
    Guideline of best practices to follow to configure Github Enterprise Cloud in a secure way.
    <br />
    <br />
    <a href="https://github.com/dduzgun-security/github-enterprise-cloud-configuration/issues">Report an issue</a>
  </p>
</p>


## Table of contents

<!--ts-->
   * [About the project](#about-the-project)
   * [Enterprise account section](#enterprise-account-section)
   * [Organization section](#organization-section)
     * [Profile settings](#organization-profile-settings)
     * [Member privileges](#member-privileges)
     * [Billing manager](#billing-manager)
     * [Organization security](#organization-security)
     * [Verify domain](#verify-domain)
     * [Audit log](#audit-log)
     * [Organization webhooks](#organization-webhooks)
     * [Organization third party access](#organization-third-party-access)
     * [Organization OAuth apps](#organization-oauth-apps)
     * [Organization GitHub apps](#organization-github-apps)
     * [Organization owner](#organization-owner)
     * [Blocked users](#blocked-users)
     * [Terms of service](#terms-of-service)
   * [User section](#user-section)
     * [User creation](#user-creation)
     * [User security](#user-security)
   * [Repository section](#repository-section)
     * [Repository creation](#repository-creation)
     * [Naming convention](#naming-convention)
     * [Tagging convention](#tagging-convention)
     * [Repository template](#repository-template)
     * [Manage access](#manage-access)
     * [Branch protection](#branch-protection)
     * [Repository webhook](#repository-webhook)
     * [Repository notification](#repository-notification)
     * [Repository integration and services](#repository-integration-and-services)
     * [Repository deploy key](#repository-deploy-key)
     * [Repository autolink references](#repository-autolink-references)
     * [Repository secrets](#repository-secrets)
     * [Security policy and advisories](#security-policy-and-advisories)
     * [Security alerts](#security-alerts)
     * [Contributing file](#contributing-file)
     * [README file](#readme-file)
     * [CODEOWNER file](#codeowner-file)
     * [GITIGNORE file](#gitignore-file)
     * [LICENSE file](#license-file)
     * [GitHub Actions](#github-actions)
     * [GitHub Packages](#github-packages)
     * [Repository-forking](#repository-forking)
     * [Repository duplicating](#repository-duplicating)
     * [Open sourcing](#open-sourcing)
     * [Agility](#agility)
   * [GitHub API section](#github-api-section)
     * [GitHub Rest API](#github-rest-api)
     * [GitHub GraphQL API](#github-graphql-api)
   * [Compliancy section](#compliancy-section)
   * [Support section](#support-section)
   * [Contributing](#contributing)
   * [License](#license)
   * [Contact](#contact)
<!--te-->

<!-- ABOUT THE PROJECT -->
## About the project
Looking for a guideline to configure your GitHub Enterprise Cloud in a secure way? 

Here is a :fire: list of things to do!

<!-- ENTERPRISE ACCOUNT SECTION -->
## Enterprise account section

<!-- ORGANIZATION SECTION -->
## Organization section
##### Profile settings
Provide only necessary profile information
```
Organization display name
Description
URL
Location
Picture
```

##### Member privileges
|               | Status        | Description  |
| ------------- |:-------------:| -----:       |
| Base permissions | None | Members will only be able to clone and pull public repositories. To give a member additional access, you’ll need to add them to teams or make them collaborators on individual repositories. |
| Repository creation | Internal | Members will be able to create private repositories, visible to organization members with permission. |
| Repository forking | Unchecked | Refer to the [repository forking section](#repository-forking). |
| Actions | Enable local Actions for this organization | This allows all repositories to execute any Action, whether the code for the Action exists within the same repository, same organization, or a repository owned by a third party. |
| Repository visibility changes | Unchecked | Only organization owners can change repository visibilities. Or, [repository creation and settings](#repository-creation) must be done as code via a PR |
| Repository deletion and transfer | Unchecked | Only organization owners can delete or transfer repositories. Or, [repository creation and settings](#repository-creation) must be done as code via a PR |
| Issue deletion | Unchecked | Only organization owners can delete issues. Or, [repository creation and settings](#repository-creation) must be done as code via a PR |
| Repository comments | Unchecked | No real need to see comment author’s profile name in issues and pull requests for private repositories. |
| Tream creation | Unchecked | Only organization owners can create new teams. Or, [repository creation and settings](#repository-creation) must be done as code via a PR. |
| Outsider collaborators | Disabled | No [outsider collaborators](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/setting-permissions-for-adding-outside-collaborators) should be able to collaborate in the organization. |

##### Billing manager
A [billing manager](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/adding-a-billing-manager-to-your-organization) for the organization must be defined.

##### Organization security
|               | Status        | Description  |
| ------------- |:-------------:| -----:       |
| Two factor authentication :lock: | Enabled |Require [two-factor authentication](https://help.github.com/en/github/authenticating-to-github/securing-your-account-with-two-factor-authentication-2fa) for everyone in the organization. |
| SAML single sign-on :lock: | Enable | [Enable SAML authentication](https://help.github.com/en/github/setting-up-and-managing-your-enterprise-account/enforcing-security-settings-in-your-enterprise-account#enabling-saml-single-sign-on-for-organizations-in-your-enterprise-account) for the organization through an identity provider like Azure AD. [SSO authentication to GitHub](https://help.github.com/en/github/authenticating-to-github/about-authentication-with-saml-single-sign-on) must be from a known internal identity provider. |
| SSO on access tokens :lock: | Enable | [Enable SSO for the use of access tokens](https://help.github.com/en/github/authenticating-to-github/authorizing-a-personal-access-token-for-use-with-saml-single-sign-on) within the organization. |
| SSO on SSH keys :lock: | Enable | [Enable SSO for the use of SSH keys](https://help.github.com/en/github/authenticating-to-github/authorizing-an-ssh-key-for-use-with-saml-single-sign-on) within the organization.
| SCIM integration :lock: | Enable | With SCIM, administrators can automate the exchange of user identity information between systems to add, manage and remove organization members. |
| SSH certificate authorities :lock: | Enable | [Manage the organization SSH CA](https://help.github.com/en/github/setting-up-and-managing-your-enterprise-account/enforcing-security-settings-in-your-enterprise-account#managing-your-enterprise-accounts-ssh-certificate-authorities) to require the members to have [SSH CA](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/about-ssh-certificate-authorities) certificates to access organization resources.  |
| Automated security update :lock: | Enable (unchecked) | Automatic security checks for dependencies and trigger a pull request in the vulnerable repository with the [automated security update](https://help.github.com/en/github/managing-security-vulnerabilities/configuring-automated-security-updates) feature. |

##### Verify domain
The organization administrator must add the TXT record to our DNS configuration in order to be a [Verified Organization Domain in GitHub](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/verifying-your-organizations-domain). This control will also restrict email notifications to an approved domain to prevent information leak

##### Audit log
The SOC team must be able to use the GitHub audit log API to do further security analysis and investigation on events from the [audit log](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/reviewing-the-audit-log-for-your-organization#using-the-audit-log-api). 


##### Organization webhooks
Every webhooks that the GitHub organization owners wants to add must be reviewed and approved by the security team.

##### Organization third party access
Every third-party access that the GitHub organization owners wants to add must be reviewed and approved by the security team.

##### Organization OAuth apps
Every GitHub Oauth apps that the GitHub organization owners wants to add must be reviewed and approved by the security team.

##### Organization GitHub apps
Every GitHub Apps that the GitHub organization owners wants to add must be reviewed and approved by the security team.

##### Organization owner
[GitHub organization owners](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/permission-levels-for-an-organization) account can basically do everything, and this is the reason why we must limit to a max of 3-5 users.

##### Blocked users
This feature must be called from an API if the SOC team detects strange events going on from a users account. We can [block a user from within your organization](https://help.github.com/en/github/building-a-strong-community/blocking-a-user-from-your-organization) settings or from a specific comment made by the user.

##### Terms of service
The Standard Terms of Service is an agreement between GitHub and you as an individual. To enter into an agreement with GitHub on behalf of an entity, such as a company, non-profit, or group, organization owners can upgrade to the [Corporate Terms of Service](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/upgrading-to-the-corporate-terms-of-service).

<!-- USER SECTION -->
## User section
##### User creation
User creation must be done by using the corporate email.

##### User security
As listed in this README file, follow all the [GitHub Account security best practices](https://github.com/security).

<!-- REPOSITORY SECTION -->
## Repository section
##### Repository creation
The repository creation must be initiated from a pull request.
  
Once the PR is accepted, it will create a new repository respecting all the rules listed in this section.

##### Naming convention
Repositories must have an effective naming conventions to help scale the organization for long term.
  
*Microservices*
``` 
<method>-<project-name>-<communication>-service 
ex: get-authentication-rest-service
ex: post-authorization-grpc-service
```
  
*Libraries*
``` 
<project-name>-library
ex: validation-library
ex: error-library
ex: jwt-library
```

##### Tagging convention
GitHub repositories must be tagged with the `ApplicationID`, `Technology used` and other usefull informations.

##### Repository template
To have a complete repository standardisation, we must use the created [GitHub Template Repository](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-template-repository)  
  
With templates repositories, we can reuse organization based :
* `README.md` files template
* `SECURITY.md` files template
* `LICENSE.txt` files template
* `CONTRIBUTING.md` files template
* `CODEOWNER` file template
* `GITIGNORE` file template
* `OPENAPI` file template
  
Also, we can use this feature to create basic reusable codegen templates for :
* Organization base level GitHub Actions
    * Code quality (Codacy, SonarCloud, ...)
    * Dependency checks (Snyk, Dependabot, ...)
    * Secret scanning
    * Docker image security scaning
    * GitHub Package publishing (with versioning)
* Microservices
    * Code template with clean architecture respecting the [SOLID principle](https://scotch.io/bar-talk/s-o-l-i-d-the-first-five-principles-of-object-oriented-design) and [12 Factor Application](https://12factor.net/).
* Libraries

##### Manage access
|               | Status        | Description  |
| ------------- |:-------------:| -----:       |
| With teams | :heavy_check_mark: | Members are part of teams and every access to repositories must be given with teams. |
| With individual members | :x: | Individual members must not be able to be added to repositories. They must be a part of a team. |
| With outside collaborators | :man_technologist: | For **open-source projects only**, the community must agree the terms and conditions of the company. |

##### Branch protection
|               | Status        | Description  |
| ------------- |:-------------:| -----:       |
| Branch name | master | The master branch must always be protected. |
| Require pull request before merging | :heavy_check_mark: | A pull request must absolutely be made before merging to master. No direct push to master must be allowed. |
| Require 2 approving reviews | :heavy_check_mark: | Minimum 2 approved reviews should be passed and for major changes with security modification, a security advisor must be assigned in the pull request. |
| Require review from Code Owner | :heavy_check_mark: | When possible, the code owner must review the pull requests. |
| Require status check to pass before merging | :heavy_check_mark: | The new feature branch must pass all the required checks in order to be merged to master. Otherwise the merge would be impossible in order to protect the master branch. |
| Require the branch to be up to date before merging | :heavy_check_mark: | The branches must be up to date before merging to avoid git conflicts. |
| Required signed commits | :heavy_check_mark: | Only [Signed Commits](https://help.github.com/en/github/authenticating-to-github/managing-commit-signature-verification) must be allowed. [Here](https://help.github.com/en/github/authenticating-to-github/signing-commits) is a How-to.|
| Include restrictions to administrators | :heavy_check_mark: | Administrators must not be able to bypass the branch security settings; they need to do pull requests also. |

##### Repository webhook
Every Webhooks that the GitHub repository owners wants to add must be reviewed and approved by the security team.

##### Repository notification
Every Notifications that the GitHub repository owners wants to add must be reviewed and approved by the security team.

##### Repository integration and services
:warning: This section is deprecated, do not use it.

##### Repository deploy key
Every Deploy Keys that the GitHub repository owners wants to add must be reviewed and approved by the security team. Deploy keys must respect the least privilege principle to [access](https://developer.github.com/v3/guides/managing-deploy-keys/) a repository.

##### Repository autolink references
Every Autolink references that the GitHub repository owners wants to add must be reviewed and approved by the security team.

##### Repository secrets
GitHub repositories must not have any clear text secrets in the code, the secrets must be in the [GitHub Secrets](https://help.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets).
  
We must call [HashiCorp Vault](https://www.vaultproject.io/) to retrieve secrets. To do so, we must use the [GitHub Auth Method](https://www.vaultproject.io/docs/auth/github/). Since GitHub is synched to the companies Identity Provider, it can easily and securely become our source of access to HashiCorp Vault. We must therefore ensure that secure [HashiCorp Vault Policies](https://learn.hashicorp.com/vault/identity-access-management/iam-policies) are bulletproof.

##### Security policies and advisories
Repositories must have a [GitHub Security Policy](https://help.github.com/en/github/managing-security-vulnerabilities/adding-a-security-policy-to-your-repository) document `SECURITY.md` to give instructions on how to responsibly report a security vulnerability.  
  
[GitHub Security Advisories](https://help.github.com/en/github/managing-security-vulnerabilities/about-github-security-advisories) must be used to privately discuss, fix, and publish information about security vulnerabilities.

##### Security alerts
We must ensure that [GitHub Automated Security Alerts](https://help.github.com/en/github/managing-security-vulnerabilities/configuring-automated-security-updates) is enabled to have less security issues in our code.

##### Contributing file
We must have a `CONTRIBUTING.md` file that describes the steps to follow and respect in order to contribute to a project.

##### README file
Each repository must have a clear `README.md` file that describes the goal of the repository and helps developers to set up their working environment quickly.

##### CODEOWNER file
Each repository must have a CODEOWNER file to request reviews to the code owner on pull request.

##### GITIGNORE file
Each repository must have a GITIGNORE file to ignore sensible information or garbage data generated by IDEs.

##### LICENSE file
Each repository must have a `LICENSE.txt` file that is **approved by the legal team** of the organization. We must have specific licensing rules in repositories (private, public, ...)

##### GitHub Actions
[GitHub Actions](https://github.com/features/actions) must be reusable and bulletproof. Therefore, they should be reviewed by **DevOps**, **CloudOps** and **SecOps** teams.  
  
Actions must ease automation tasks in testing, deployment, security, notification and many other fields.  
  
The template repository must have the base level GitHub Actions set by the organization at each time.

##### GitHub Packages
We must use as much as possible the [GitHub Packages](https://github.com/features/packages) as our code and docker image registry to have an all-in-one tool that requires no external connections to other tools such as Nexus or JFrog.  
  
In order to always have deployed artefact and images that are regulary scanned, we must add restriction in Github Actions when pulling a version of Package that is old. Github Packages :package: older then 3 months must not be consumed for security reasons.  
  
To re-run the GitHub Actions in order to have a new Github Package version simply push a change in the `README.md` file that doesn’t affect the code and then the base level action will follow all the security checks.

##### Repository forking
Members will be able to [fork](https://help.github.com/en/github/getting-started-with-github/fork-a-repo) public (open source) repositories. When a member decides to fork a repository, he/she must ensure that he/she will merge his/her changes to the open source project. No sensible information must be in the forked code since repositories that are forked are by default public in GitHub.

##### Repository duplicating
Members who would like to add sensible data on existing open source projects must [duplicate the repository](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/duplicating-a-repository) and make it private within the organization instead of forking it.

##### Open sourcing
Yes! :sunglasses:	
  
If the code doesn't leak sensible data and is reviewed by the security team, we strongly encourage you to make it [open-source](https://github.com/open-source) by including the licensing and going through the legal approbations of the organization.

##### Agility
We strongly encourage teams to use GitHub to work in an agile way. :+1:
  
* [Create Issues](https://help.github.com/en/github/managing-your-work-on-github/creating-an-issue)
* [Create Project Boards](https://help.github.com/en/github/managing-your-work-on-github/creating-a-project-board)
* [Assign Reviewers](https://help.github.com/en/github/managing-your-work-on-github/assigning-issues-and-pull-requests-to-other-github-users)
* [Create Milestones](https://help.github.com/en/github/managing-your-work-on-github/about-milestones) 
* and [more](https://help.github.com/en/github/managing-your-work-on-github).

<!-- GITHUB API SECTION -->
## GitHub API section
##### GitHub Rest API
[GitHub Rest API](https://developer.github.com/v3/) helps to configure many things in GitHub such as inviting/removing members, creating repositories and more.

##### GitHub GraphQL API
[GitHub GraphQL API](https://developer.github.com/v4/) is also available and it offers the ability to define pricisely only the required data you want in a single call. It is also very usefull to [audit GitHub logs](https://github.blog/2019-06-21-the-github-enterprise-audit-log-api-for-graphql-beginners/).

:warning: Keep note of the rate limiting in place when doing HTTP requests.

<!-- COMPLIANCY SECTION -->
## Compliancy section
No more need to worry about server deployments, updates, availability, hardening or other because with the cloud-hosted [GitHub Enterprise Cloud](https://help.github.com/en/github/getting-started-with-github/setting-up-a-trial-of-github-enterprise-cloud#exploring-github-enterprise-cloud) because they handle this part.

* [Security trust, audit and compliancy](https://github.com/security/trust)
* [Policies](https://help.github.com/en/github/site-policy/github-enterprise-cloud-addendum)
* [Incident response](https://github.com/security/incident-response)
* [Security advisory](https://github.com/advisories)
* [Bug bounty program](https://bounty.github.com/)




<!-- SUPPORT SECTION -->
## Support section
[Github Enterprise Support](https://enterprise.github.com/support) offers very usefull assistance on everything you search. :+1:

* [Documentations](https://help.github.com/en)
* [Request creation](https://enterprise.githubsupport.com/hc/en-us/requests/new)

Also, GitHub offers a [Premium Support](https://help.github.com/en/github/working-with-github-support/about-github-premium-support-for-github-enterprise-cloud) with a 24/7 hours of operation availability time.




<!-- CONTRIBUTING -->
## Contributing
Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<!-- LICENSE -->
## License
Distributed under the MIT License. See `LICENSE.txt` for more information.

<!-- CONTACT -->
## Contact
[Deniz Onur Duzgun](https://github.com/dduzgun-security)  
[Khalid Nazmus Sakib](https://github.com/knsakibnbc)


<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/dduzgun-security/github-enterprise-cloud-configuration.svg?style=flat-square
[contributors-url]: https://github.com/dduzgun-security/github-enterprise-cloud-configuration/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/dduzgun-security/github-enterprise-cloud-configuration?style=flat-square
[forks-url]: https://github.com/dduzgun-security/github-enterprise-cloud-configuration/network/members
[stars-shield]: https://img.shields.io/github/stars/dduzgun-security/github-enterprise-cloud-configuration.svg?style=flat-square
[stars-url]: https://github.com/dduzgun-security/github-enterprise-cloud-configuration/stargazers
[issues-shield]: https://img.shields.io/github/issues/dduzgun-security/github-enterprise-cloud-configuration.svg?style=flat-square
[issues-url]: https://github.com/dduzgun-security/github-enterprise-cloud-configuration/issues
[license-shield]: https://img.shields.io/github/license/dduzgun-security/github-enterprise-cloud-configuration.svg?style=flat-square
[license-url]: https://github.com/dduzgun-security/github-enterprise-cloud-configuration/blob/master/LICENSE.txt