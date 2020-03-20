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
   * [Repository section](#repository-section)
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
     * [GitHub Actions](#github-actions)
     * [GitHub Packages](#github-packages)
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

<!-- REPOSITORY SECTION -->
## Repository section
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

##### GitHub Actions
[GitHub Actions](https://github.com/features/actions) must be reusable and bulletproof. Therefore, they should be reviewed by **DevOps**, **CloudOps** and **SecOps** teams.  
  
Actions must ease automation tasks in testing, deployment, security, notification and many other fields.  
  
The template repository must have the base level GitHub Actions set by the organization at each time.

##### GitHub Packages
We must use as much as possible the [GitHub Packages](https://github.com/features/packages) as a code and docker image registry to have an all-in-one tool that requires no external connections to other tools such as Nexus or JFrog.  
  
In order to always have deployed artefact and images that are regulary scanned, we must add restriction in Github Actions when pulling a version of Package that is old. Packages older that 3 months must not be consumed.  
  
To re-run the GitHub Actions in order to have a new Github Package version simply push a change in the `README.md` file that doesn’t affect the code and then the base level action will follow all the security checks.

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