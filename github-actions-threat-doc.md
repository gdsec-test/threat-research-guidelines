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
- Before Step 3, make sure you haev required access for your repository, for `Actions` to be visible under 'Settings' tab. If you working on forked repos/ make sure the access is granted under `Actions` tab on top.
- Follow instructions for Step 3
- For Step 4, before running the `config.sh`, change directory and switch role

   ```sh 
       cd /usr/local/bin/actions-runner 
       sudo su github-actions
    ```
    - Run the `config.sh` command
    - You will be prompted to `sudo` install some dependency. Switch role back to your user(not `github-actions`) to perform that and switch again to `github-actions` for `config.sh`
     
