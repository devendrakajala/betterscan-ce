[<img src="https://img.shields.io/discord/953265912302141460?label=Discord%20Chat">](https://discord.gg/3pvz7Tx9Zz) ![GitHub stars](https://badgen.net/github/stars/marcinguy/scanmycode-ce)
![GitHub forks](https://badgen.net/github/forks/marcinguy/scanmycode-ce)
![GitHub watchers](https://badgen.net/github/watchers/marcinguy/scanmycode-ce)
![GitHub issues](https://badgen.net/github/issues/marcinguy/scanmycode-ce)
![Docker Pulls](https://badgen.net/docker/pulls/scanmycode/scanmycode3-ce?icon=docker&label=pulls)

If you want to scan your Code and Infrastructure (including Secrets)

Install  Docker Engine [(Instructions for Ubuntu)](https://docs.docker.com/engine/install/ubuntu/), if you don't already have it, and run this in your Git code directory

**2 options** are available:

1. HTML output (result will be in the current directory in  "report.html" file)

Run in command prompt:

`sh <(curl https://dl.betterscan.io/cli-html.sh)`

2. CLI output

Run in command prompt:

`sh <(curl https://dl.betterscan.io/cli.sh)`

If you need CI/CD and Web Interface, you need Docker-Compose [(Instructions for Ubuntu)](https://docs.docker.com/compose/install/) installed as well, if you don't already have it.

Run in command prompt:

```
git clone https://github.com/marcinguy/betterscan-ce.git
cd betterscan-ce/dockerhub
./start.sh
```


Open up the Browser to:

`http://localhost:5000`

Sign up locally (and login in when needed)


That's it.

Read more below for GitHub/GitLab/Azure DevOps Server integration, PR scanning, GitHub Action, GitHub App etc.

# What it does

Scanmycode is now called Betterscan (both references will work)

It is a Code and Infrastructure (IaC) and Cloudnative Scanning/SAST/Static Analysis/Linting solution using many tools/Scanners with One Report. You can also add any tool to it. Currently, it supports many languages and tech stacks. Similar to SonarQube, but it is different.


![betterscan-concept](https://user-images.githubusercontent.com/20355405/201513479-fda1db4d-ce99-4063-9f55-329885c6f809.png)


*Fig. 1 Betterscan concept diagram*

[How is Betterscan different than SonarQube?](#how-is-betterscan-different-than-sonarqube)

If you like it, please give it a GitHub star/fork/watch/contribute. This will ensure continous development :star:

## Sponsors

This project would not be possible without the generous support of our sponsors.

<table>
  <tr>
    <td>Your Logo Here</td>
    <td>Your Logo Here</td>
    <td>Your Logo Here</td>
    <td>Your Logo Here</td>
  </tr>
</table>

If you also want to support this project, head over to our [Github sponsors page](https://github.com/sponsors/marcinguy) or [Patreon](https://www.patreon.com/marcinguy) (preferred due to better Tax handling)

# TL;DR


Run this command in your code directory (checkout from Git - .git folder needs to be there, if you work with the normal directory, make the Git repo out of it `git init && git add . && git commit` etc):

`sh <(curl https://dl.betterscan.io/cli.sh)`

or for an HTML report:

`sh <(curl https://dl.betterscan.io/cli-html.sh)`

That's it. You just checked your code for 2,300+ Checks (Defects, Vulnerabilities, Best Practices, Secrets - 166+ secret types - including GitLeaks) and 4,000+ YARA rules for Antidebug, Antivm, Crypto, CVE, Exploits Kits, Malware, Webshells, APTs.

Sample CLI report

![betterscan-results-cli](https://user-images.githubusercontent.com/20355405/184548807-a13f5943-96ed-4c64-bf73-c4afdabd86d5.png)


Sample HTML report

![image](https://user-images.githubusercontent.com/20355405/173091087-1edf7609-5006-4724-b46b-bab2502bc251.png)

*Fig 2. Sample Report using CLI HTML output*

FYI Above will maintain state via (.checkmate folder), only new commits will be checked.

If you want to use the Platform, follow below:


## Local

<h4>To install it, first:</h4>

Install `docker` and `docker-compose`  
 
<h5>then:</h5>

<strong>2 options:</strong>

1) Fastest method (use DockerHub built images). If unsure, use this.

```
git clone https://github.com/marcinguy/betterscan-ce.git
cd betterscan-ce/dockerhub
./start.sh
```

2) Slower method (build everything)

```
git clone https://github.com/marcinguy/betterscan-ce.git
cd betterscan-ce/docker
./start.sh
```

Open up the Browser to:

`http://localhost:5000`

Sign up locally (and login in when needed)

## Cloud

Cloud and Kubernetes (scaling) installation:

If your connection is not fast and/or you have no server.

Installation time: ca. 1 minute

Check installation on Kubernetes (Free) thanks to Okteto.com

https://github.com/marcinguy/betterscan-ce/blob/master/okteto/README.md

## GitHub Action

WIP (Work in Progress)

Add this to your workflow (in your GitHub repo under .github/workflows/betterscan.yml) with this content:


```
name: Betterscan Scan
on: [push]
jobs:
  Betterscan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - name: Betterscan Scan
        uses: topcodersonline/betterscan@v3
      - name: Upload the SARIF file
        uses: github/codeql-action/upload-sarif@v2
        with:
          sarif_file: report.sarif
```

Results will be printed in Job/Action output. Plan to add SARIF output to integrate it in GitHub Code Scanning Interface/tab (possibly in PRO version only) 

See sample integration below:

![image](https://user-images.githubusercontent.com/20355405/180461548-8fbb6c47-0314-4f37-90f1-4292f788caf3.png)

*Fig 3. Sample integration with GitHub Action in Code Scanning tab*

If you want to scan on PR to main, use this:

```
name: Betterscan Scan
on: 
 pull_request:
   types: [opened, edited, reopened, review_requested, synchronize]
   branches:
      - 'main'
jobs:
  Betterscan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - name: Betterscan Scan
        uses: topcodersonline/betterscancustom@v1
        with:
         branch: ${{ github.head_ref || github.ref_name }}
      - name: Upload the SARIF file
        uses: github/codeql-action/upload-sarif@v2
        with:
          sarif_file: report.sarif
          
```

Action source:
https://github.com/topcodersonline/betterscancustom

Feel free to adjust.

Flow:
1) Make PR
2) It will scan
3) make git pull to get state (.checkmate folder) on your PR
4) make changes, commits, push to PR
5) repeat 2)

If you cannot afford GitHub Enterprise with Code Scanning, use GitHub Teams with CLI output.

It is also possible to use Database Server (i.e PostgreSQL, MySql, MariaDB, Oracle, MS Sql Server) to store state (no need to commit .checmate folder to code).

## GitHub App

You can also install it as a GitHub App

https://github.com/apps/betterscan-code-scan

![image](https://user-images.githubusercontent.com/20355405/201154860-4eac4a2b-8fea-4ba3-b38b-728a5bac5588.png)


Results will be integrated in GitHub interface (Security->Code Scanning alerts). GitHub Enterprise Cloud and a license for GitHub Advanced Security is required for private repositories, for public repositories not.

Scanning is triggered on Push/PR merge to main branch (master or main etc). Results could be there within minutes or hours, depending on project size.

Scan state will be preserved between scans. With new scan only changes will be rescanned.

## GitLab Integration

It is possible to integrate results also in GitLab's Security & Compliance Dashboad. We can convert SARIF to GitLab format. GitLab Ultimate is required. 

## Backstage Integration (BETA)

You can see Security posture of scanned repos by installing Betterscan platform and below in Backstage:

https://github.com/marcinguy/backstage-plugin-betterscan


## Azure DevOps Integration

To integrate BetterScan with Azure DevOps, you can do the following:
1. Install the Azure DevOps [SARIF SAST Scans Tab](https://marketplace.visualstudio.com/items?itemName=sariftools.scans) extension.
2. Add this job to your azure-pipelines.yml:
```yml
  - job: SAST
    displayName: Static Application Security Test (SAST)
    condition: eq(variables['Build.SourceBranch'], 'refs/heads/main')
    pool:
      vmImage: 'ubuntu-latest'
    container: 'scanmycode/scanmycode3-ce:worker-cli'
    steps:
    - script: |
        sudo apt-get update
        sudo apt-get install git-lfs
      displayName: Install git LFS
      
    - checkout: self
      persistCredentials: true

    - script: |
        set -e
        git config --global user.email "azuredevops@microsoft.com"
        git config --global user.name "Azure DevOps" 
        git checkout -b $(Build.SourceBranchName)
        sudo CODE_DIR=$(Build.SourcesDirectory) checkmate init
        sudo CODE_DIR=$(Build.SourcesDirectory) checkmate git init
        sudo CODE_DIR=$(Build.SourcesDirectory) checkmate git analyze --branch $(Build.SourceBranchName)
        sudo CODE_DIR=$(Build.SourcesDirectory) checkmate issues html
      displayName: Static Application Security Test (SAST)
      env:
        CODE_DIR: '$(Build.SourcesDirectory)'

    - task: PublishBuildArtifacts@1
      displayName: Publish SAST report
      inputs:
        PathtoPublish: $(Build.SourcesDirectory)
        ArtifactName: CodeAnalysisLogs

    - script: |
        git add .checkmate/db.sqlite
        git add report.html
        git commit -m '[ci skip] update checkmate db'
        git push origin $(Build.SourceBranchName):$(Build.SourceBranch)
      displayName: Commit and Push checkmate db
```
> Warning: 

## Platforms & OS'es

It is platform independent (Python). Checkers are also mostly available on different platforms. The "Master" branch is for Linux x86_64, however there is also a "macos" branch with Dockerfiles for arm64 (including arm64 checkers). M1 mac has arm64 architecture (30% cheaper and 30% faster than alternatives) 

### Linux (amd64)

Yes, by default 

### MacOs (Intel and arm64)

macos branch

### Windows (amd64)

Install Windows 10, version 1903 or higher or Windows 11.

https://docs.docker.com/desktop/windows/wsl/

Linux images should work

## Usage

More info in the Wiki:

https://github.com/marcinguy/betterscan-ce/wiki

## Advanced Usage (CLI Mode only)

You can plug it anywhere on your CI/CD pipeline as a command.

With Betterscan one command, you add 6,300+ checks using different scanners (Betterscan is a "Meta" scanner in that sense, with supporting smart snapshots and other goodies. It is not just running tools always on full code) 

Betterscan supports also CLI only mode, no Web Interface, worker etc. Run a binary in Docker in your own CI/CD pipeline (whatever it is) in Quality Gates that will output line by line (scanner and findings) on checkout code from Git (folder) 

![photo_2022-05-10_19-16-07](https://user-images.githubusercontent.com/20355405/167685447-84ba2b50-26fc-4143-9bb2-987ccd5e3a92.jpg)

*Fig 4. Sample CI/CD Pipeline (Photo courtesy of Viking from THC Telegram Channel)*

You can put it under Quality Gates.

### Quick Install


#### Plain CLI output

Just run this command (it will take care of everything):


`sh <(curl https://dl.betterscan.io/cli.sh)`

Corresponds to running these:

```
export CODE_DIR=${PWD}
cd $CODE_DIR
docker run -e CODE_DIR -v ${PWD}:${PWD}  -ti  scanmycode/scanmycode3-ce:worker-cli /bin/sh -c 'cd $CODE_DIR && checkmate init'
docker run -e CODE_DIR -v ${PWD}:${PWD}  -ti  scanmycode/scanmycode3-ce:worker-cli /bin/sh -c 'cd $CODE_DIR && checkmate git init'
docker run -e CODE_DIR -v ${PWD}:${PWD}  -ti  scanmycode/scanmycode3-ce:worker-cli /bin/sh -c 'cd $CODE_DIR && checkmate git analyze --branch `git rev-parse --abbrev-ref HEAD`'
docker run -e CODE_DIR -v ${PWD}:${PWD}  -ti  scanmycode/scanmycode3-ce:worker-cli /bin/sh -c 'cd $CODE_DIR && checkmate issues'
```

#### HTML CLI output

Just run this command (it will take care of everything):

`sh <(curl https://dl.betterscan.io/cli-html.sh)`

report will be in the directory under `report.html`


Corresponds to running these:

```
export CODE_DIR=${PWD}
cd $CODE_DIR
docker run -e CODE_DIR -v ${PWD}:${PWD}  -ti  scanmycode/scanmycode3-ce:worker-cli /bin/sh -c 'cd $CODE_DIR && checkmate init'
docker run -e CODE_DIR -v ${PWD}:${PWD}  -ti  scanmycode/scanmycode3-ce:worker-cli /bin/sh -c 'cd $CODE_DIR && checkmate git init'
docker run -e CODE_DIR -v ${PWD}:${PWD}  -ti  scanmycode/scanmycode3-ce:worker-cli /bin/sh -c 'cd $CODE_DIR && checkmate git analyze --branch `git rev-parse --abbrev-ref HEAD`'
docker run -e CODE_DIR -v ${PWD}:${PWD}  -ti  scanmycode/scanmycode3-ce:worker-cli /bin/sh -c 'cd $CODE_DIR && checkmate issues html'
```

### Detailed explanation


Build Docker image Worker-CLI and run `checkmate` from there. Below is sample flow:


CLI Mode only

1) Clone the repo i.e into `/tmp/test`
2) set env var CODE_DIR i.e `export CODE_DIR=/tmp/test`. This env var should point to your Git cloned repo dir. 
3) Cd into it (this is important!)
4) Run `checkmate init`
5) Run `checkmate git init`
6) Run `checkmate git analyze`


Run `checkmate issues`

This will be shown
```
/root
/tmp/test
/tmp/test
Loading plugin: git
Loading plugin: trufflehog3
Loading plugin: trojansource
Loading plugin: metrics
Loading plugin: bandit
Loading plugin: brakeman
Loading plugin: phpanalyzer
Loading plugin: gosec
Loading plugin: confused
Loading plugin: pmd
Loading plugin: semgrep
Loading plugin: semgrepdefi
Loading plugin: semgrepjs
Loading plugin: checkov
Loading plugin: kubescape
Loading plugin: insidersecswift
Loading plugin: insiderseckotlin
Loading plugin: insiderseccsharp
Loading plugin: pmdapex
Loading plugin: semgrepccpp
Loading plugin: semgrepjava
Loading plugin: semgrepeslint
semgrepjs	ExpressLfrWarning	
semgrepjs	CookieSessionNoDomain	
semgrepjs	CookieSessionNoPath	
semgrepjs	CookieSessionNoSecure	
semgrepjs	CookieSessionDefault	
semgrepjs	CookieSessionNoSamesite
```

There is a DockerHub image also for it ready.

`docker pull scanmycode/scanmycode3-ce:worker-cli`

You can run the Commands with docker as below:

```
$ docker run -ti  scanmycode/scanmycode3-ce:worker-cli checkmate
/root
/root
Loading plugin: git
Loading plugin: trufflehog3
Loading plugin: trojansource
Loading plugin: metrics
Loading plugin: bandit
Loading plugin: brakeman
Loading plugin: phpanalyzer
Loading plugin: gosec
Loading plugin: confused
Loading plugin: pmd
Loading plugin: semgrep
Loading plugin: semgrepdefi
Loading plugin: semgrepjs
Loading plugin: checkov
Loading plugin: kubescape
Loading plugin: insidersecswift
Loading plugin: insiderseckotlin
Loading plugin: insiderseccsharp
Loading plugin: pmdapex
Loading plugin: semgrepccpp
Loading plugin: semgrepjava
Loading plugin: semgrepeslint
Usage: checkmate [command] [command] [...] [args]

Type "checkmate help" for help
```

Same workflow as above, but using Docker binary:

```
export CODE_DIR=/tmp/test
cd /tmp/test
docker run -e CODE_DIR -v /tmp/test:/tmp/test  -ti  scanmycode/scanmycode3-ce:worker-cli /bin/sh -c 'cd /tmp/test && checkmate init'
docker run -e CODE_DIR -v /tmp/test:/tmp/test  -ti  scanmycode/scanmycode3-ce:worker-cli /bin/sh -c 'cd /tmp/test && checkmate git init'
docker run -e CODE_DIR -v /tmp/test:/tmp/test  -ti  scanmycode/scanmycode3-ce:worker-cli /bin/sh -c 'cd /tmp/test && checkmate git analyze'
docker run -e CODE_DIR -v /tmp/test:/tmp/test  -ti  scanmycode/scanmycode3-ce:worker-cli /bin/sh -c 'cd /tmp/test && checkmate issues'
```


# Under the hood

Progpilot, PMD, Bandit, Brakeman, Gosec, confused, semgrep, trufflehog3, jshint, njsscan, log4shell via custom semgrep rule, checkov, kubescape, graudit, insidersec, flawfinder, find sec bugs, eslint, YARA and other(s). Some were modified. 

# Recorded DEMO

Community Edition does not have GitHub support and other plugins. But rest is the same.

![betterscan-demo](https://user-images.githubusercontent.com/20355405/152678316-04fdcd54-73e8-42f8-9bf2-fb9a69618ff9.gif)

or Check the:

[Live Demo](https://app.betterscan.io/explore)

### How is Betterscan different than SonarQube?

Both use static analysis to find bugs and defects, but there are a few differences. 

- Betterscan supports Cloudnative and Infrastructure Scanning
- Betterscan supports secrets Scanning (166+ secret types - including GitLeaks) 
- Betterscan scans for 4,000 rules for  Antidebug, Antivm, Crypto, CVE, Exploits Kits, Malware and Webshells, APTs
- Betterscan can be extended with any tool producing JSON output (any binary, in any technology/language/product)

Above are the biggest differences. 

- Betterscan is open Source, SonarQube also offers an open-source version, but it is missing features (For example, 12 of the supported languages are not available in the open-source offering, and more powerful dataflow features are only available in the paid versions) 
- Betterscan supports scanning only changed files (differential analysis), SonarQube does not. You can store state in Database (PostgreSQL, MySQL/MariaDB, Oracle, Microsoft SQL Server) or in you Git repo. 
- Outputs in CLI, HTML, SARIF, JSON. This works nicely with GitHub Codescanning, GitLab Security and Compliance and Azure DevOps Server
- Betterscan uses many tools adding up to 6,300+ checks which also semgrep as one of the tools (without semgrep community rules, only Betterscan's custom rules) 



# Even more screenshots from scanning real projects

![image](https://user-images.githubusercontent.com/20355405/154044790-a07ef065-9881-4ab6-ba05-ddf5be84e19a.png)
![image](https://user-images.githubusercontent.com/20355405/154044817-92d3ebde-45b6-4b63-a0ee-414001effbe0.png)
![image](https://user-images.githubusercontent.com/20355405/154044857-f53f1922-7e0c-4ede-ad96-b1d21075dad3.png)
![image](https://user-images.githubusercontent.com/20355405/154044887-4d69d551-9cb3-4892-85e0-1383eeab8332.png)
![image](https://user-images.githubusercontent.com/20355405/154044929-74ea5e0f-550e-4833-bd33-8d285e2195dd.png)
![image](https://user-images.githubusercontent.com/20355405/154044957-3ace283a-bb76-4f15-9f49-520d6f21e7d3.png)
![smc-defi-sample](https://user-images.githubusercontent.com/20355405/169240036-1d6652fa-0724-42cf-ac41-2bbcf7398def.png)
![smc-iac](https://user-images.githubusercontent.com/20355405/169240091-2171b1c6-9931-46be-ae97-a9f827c0120c.png)
![terra](https://user-images.githubusercontent.com/20355405/169240143-3d590a0a-bd39-4652-9556-12ccdd4563d7.png)
![image](https://user-images.githubusercontent.com/20355405/169540378-22deb07c-1665-4614-915c-511f24689438.png)



# Welcome to Betterscan CE (Community Edition)!


Betterscan is based on QuantifedCode. QuantifiedCode is a code analysis \& automation platform. It helps you to keep track of issues and
metrics in your software projects, and can be easily extended to support new types of analyses.
The application consists of several parts:

* A frontend, realized as a React.js app
* A backend, realized as a Flask app, that exposes a REST API consumed by the frontend
* A background worker, realized using Celery, that performs the code analysis

Currently supports: PHP, Java, Scala, Python, PERL, Ruby, .NET Full Framework, C#, C, C++, Swift, Kotlin, Apex (Salesforce), Javascript, Typescript, GO, Solidity, DeFi Security (DeFi exploits), Infrastructure as a Code (IaC) Security and Best Practices (Docker, Kubernetes (k8s), Terraform AWS, GCP, Azure), Secret Scanning (166+ secret types), Dependency Confusion, Trojan Source, Open Source and Proprietary Checks (total ca. 2,300+ checks) 

Advantages:
* Many tools, one report (unification) 
* Dismiss, collaborate on findings. Mark false-positives
* Enable/disable each individual check in Checkers
* ca. 6,300+ checks now (Linters, Static Code Analysis/Code Scanning, YARA Ca. 4000 YARA binary matching/textual matching rules for Antidebug, Antivm, Crypto, CVE, Exploits Kits, Malware and Webshells, APTs ) 
* any tool outputting JSON can be added
* fast (checks only new code on recheck) 
* you can store state in Database (PostgreSQL, MySQL/MariaDB, Oracle, Microsoft SQL Server) or in you Git repo.
* Outputs in CLI, HTML, SARIF, JSON. This works nicely with GitHub Codescanning, GitLab Security and Compliance and Azure DevOps Server
* Git support (HTTPS/TLS and SSH). For private repositories only SSH. 
* all REST API callable (CI/CD integrateable)
* Swiss army knife tool/SIEM for Code Scanning
* 100% Code transparency & full control of your code


Cloud version and more at https://www.betterscan.io

Cloud version has also many other plugins, also other plugins are commercially available for licensing (GitHub, GitHub organizations, Slack)

# Contribute

Looking for contributing individuals and organizations. Feel free to contact me at marcinguy@gmail.com

TODO

* update Dependencies (Backend & Frontend - done)
* update to latest React
* update to Python3 (see scanmycode3 branch - done)
* update/add new Checkers (if you wish)

# Licensing

Betterscan's QuantifiedCode parts remain released under BSD-3 Clause License. However, modifications are released under LGPL-2.1 with Commonsclause.

You can use this software, but cannot sell it, also base services on it (SaaS - Software as a Service setups). This is the Commonsclause. If you would like to do it, please contact me first for the permission at marcinguy@gmail.com

# Installation

We provide several options for installing Betterscan. Which one is the right one for you
depends on your use case.

* The **manual installation** is best if you want to modify or change Betterscan 
* The **Docker-based installation** is probably the easiest way to try Betterscan without much work
* The **Ansible-based installation** is the most suitable way if you want to run Betterscan in a professional infrastructure (possibly with multiple servers)

The following section will only discuss the manual installation process, for the other options please
check their corresponding repositories.

## Manual Installation

The installation consists of three parts:

* Install the dependencies required to run Betterscan 
* Download the required source code
* Set up the configuration

### Installing Dependencies

Betterscan requires the following external dependencies:

* A message broker (required for the background tasks message queue). We recommend either RabbitMQ or Redis.
* A database (required for the core application). We recommend PostgreSQL, but SQLite is supported as well. Other database systems might work too (e.g. MySQL), but are currently not officially supported. If you need to run Betterscan on a non-supported database, please get in touch with us and we'll be happy to provide you some guidance.

### Download the Betterscan CE source code

Now with the dependencies installed, we can go ahead and download Betterscan:

    `git clone https://github.com/marcinguy/betterscan-ce.git`


### Install the required Python packages

Betterscan CE manages dependencies via the Python package manager, pip.


### Edit Settings

Betterscan gets configured via YAML settings files. When starting up the application, it incrementally loads settings from several files, recursively updating the settings object. First, it will load default settings from `quantifiedcode/settings/default.yml`. Then, it will check if a `QC_SETTINGS` environment variable is defined and points to a valid file, and if so it will load settings from it (possibly overwriting default settings). If not, it will look for a `settings.yml` file in the current working
directory and load settings from there. Additionally, it will check if a `QC_SECRETS` environment variable is defined and points to a valid file, and also load settings from there (this is useful for sensitive settings that should be kept separate from the rest [e.g. to not check them into version control]).

There is a sample `settings.yml` file in the root of the repository that you can start from.

### Running the Setup

After editing your settings, run the setup command via
````
    #run from the root directory of the repository
    python manage.py setup
````
The setup assistant will iteratively walk you through the setup, and when finished you should have a
working instance of Betterscan!

### Running the web application

To run the web application, simply run

    `python manage.py runserver`

### Running the background worker

To run the background worker, simply run

    `python manage.py runworker`

## Docker-Based Installation

See docker folder. You can spin up everything using one command.

## Ansible-Based Installation

Coming Soon!




