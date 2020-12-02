ThreatApi code has dependencies being pulled from github.secureserver.net

We are setting up the CICD pipeline with self-hosted runners because of this dependency.

## Setting up self-hosted runners
- Follow instructions on [setting up on-prem openstack](https://github.secureserver.net/CTO/guidelines/blob/master/Standards-Best-Practices/CICD/GitHubActions.md#setting-up-a-self-hosted-github-actions-runner-on-on-prem-openstack) for points 1 & 2
  - For 2d. Run 
     ```sh
       for ip in `cat /etc/resolv.conf | grep nameserver | cut -d " " -f2`; do sudo iptables -I OUTPUT 1 -p udp -d $ip --dport 53 -m state --state NEW,ESTABLISHED -j ACCEPT;
       done;
     ```
  - Not installing docker at the moment. Can be downloaded when we are using Actions that require docker.
- Before Step 3, make sure you have required access for your repository, for `Actions` to be visible under 'Settings' tab. If you working on forked repos/ make sure the access is granted under `Actions` tab on top.
- Follow instructions for Step 3
- For Step 4, before running the `config.sh`, change directory

   ```sh 
       cd /usr/local/bin/actions-runner 
    ```
    - Run the `config.sh` command
    - You will be prompted to `sudo` install dependency
      ```sh
      sudo ./bin/installdependencies.sh
      ```
     - Switch role to perform the `config.sh`
       ```sh
       sudo su github-actions
       ```
     - Swtich back to your user role before making the git path changes
- Follow the rest of steps as is

## Starting with actions 
- Check out [gdcorp-cp/gd-actions-starter-workflows](https://github.com/gdcorp-cp/gd-actions-starter-workflows)
- For GoDaddy specific inhouse built actions checkout [gdcorp-cp/gd-actions](https://github.com/gdcorp-cp/gd-actions)

## Setting up Python build 
 - Using the `actions/setup-python` at [here](https://github.com/marketplace/actions/setup-python) for getting the environment ready 
   - Github hosted images have Python already installed 
   - Scripts for self-hosted runners are available from `@v2` - make sure you checkout from this version onwards to automatically setup for self-hosted runners
 - Make sure you use the version matrix with the [Python versions specified](https://raw.githubusercontent.com/actions/python-versions/main/versions-manifest.json) by github team
     
