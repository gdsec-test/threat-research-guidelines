ThreatApi code has dependencies being pulled from github.secureserver.net

We are setting up the CICD pipeline with self-hosted runners because of this dependency for GoLang. Rest runs on Github hosted runners.
We have a max run time of 50k billable mins/ month for entire GoDaddy. Currently, there is no restriction on using the mins as there is very less adoption of Github actions within company.


## Setting up self-hosted runners
- Follow instructions on [setting up on-prem openstack](https://github.secureserver.net/CTO/guidelines/blob/master/Standards-Best-Practices/CICD/GitHubActions.md#setting-up-a-self-hosted-github-actions-runner-on-on-prem-openstack) for points 1 & 2
  - For 2d. Run 
     ```sh
       for ip in `cat /etc/resolv.conf | grep nameserver | cut -d " " -f2`; do sudo iptables -I OUTPUT 1 -p udp -d $ip --dport 53 -m state --state NEW,ESTABLISHED -j ACCEPT;
       done;
     ```
     
## Threat Team's Self Hosted machine 
#### On Github Cloud
- Tied to Repository `gdcorp-infosec/threat-api`
- Can be found under `Settings -> Actions -> Self-hosted Runners`
- Named as `github-actions-threatapiv2`
#### On Openstack
- Available under Project `threatapi-v2-prod` 
- Named as `github-actions`

## Starting with actions 
- Check out [gdcorp-cp/gd-actions-starter-workflows](https://github.com/gdcorp-cp/gd-actions-starter-workflows)
- For GoDaddy specific inhouse built actions checkout [gdcorp-cp/gd-actions](https://github.com/gdcorp-cp/gd-actions)

## Setting up Python build 
 - Using the `actions/setup-python` at [here](https://github.com/marketplace/actions/setup-python) for getting the environment ready 
   - Github hosted images have Python already installed 
   - Scripts for self-hosted runners are available from `@v2` - make sure you checkout from this version(SHA recommended) onwards to automatically setup for self-hosted runners
 - Make sure you use the version matrix only from the [Python versions specified](https://raw.githubusercontent.com/actions/python-versions/main/versions-manifest.json) by github team
   - Python Self hosted runners are supported only for ubuntu, windows, macOS
     - See for docker containers or set up python by your own actions

## Setting up Go build 
 - Using the `actions/setup-go` at [here](https://github.com/actions/setup-go)
   - Can be run on github hosted/ self hosted unlike python dependency 
   - ThreatAPI uses self-hosted as we are pulling [threat/util](https://github.secureserver.net/threat/util) dependency. Since this repo is within GD network and code wasn't scanned with tartufo, this code cannot be checked onto github hosted runners for building. Once we move away from that dependency we can use github hosted runners.

## Staying up to date with SHA commits
 - We are using dependabot under `.github/` under our repository with `package-ecosystem: "github-actions"` to stay up to date with SHA versions for dependencies that we use. This bot creates a PR when a new SHA is detected from the dependency. (TODO: Check if the dependabot is applicable to GD repositories as well)
     
