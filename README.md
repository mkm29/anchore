# Anchore Setup

---

## Local Installation/Usage

1. Install Anchore CLI

This is done via PIP: `pip install --user --upgrade anchorecli`

  * Make sure that the command `anchore-cli` is in your PATH, if not locate it:
> `find ~ -name anchore-cli`
  * Add this file (or parent directory to shell profile)

1. Clone this repo
2. Tune config.yaml settings to your liking
3. Bring up Anchore containers
> `docker-compose up -d`

### Usage

1. For basic scans, you need to first add the image to be analyzed:
> `anchore-cli --u admin --p foobar image add quay.agilesof.com/brandi-dev/ad-hoc-service:latest`

2. Unfortanately Anchore does not inform you once an image is scanned, you can view an individual image status with:
> `anchore-cli --u admin --p foobar image get quay.agilesof.com/brandi-dev/ad-hoc-service:latest`

3. Viewing Security Vulnerabilities
The basic format is: `anchore-cli image vuln INPUT_IMAGE VULN_TYPE`
  * VULN_TYPE can either be `os`, `non-os` or `all`

4. Output to JSON
> `anchore-cli --u admin --p foobar --json image vuln quay.agilesof.com/brandi-dev/ad-hoc-service:latest all > 'cve.json'`


## docker-compose

It is much easier to use docker-compose, the yaml files are included in this directory, just run `docker-compose up -d` from this directory.

### Usage

**Note**: You can sh into the container if you do not want to preface all commands with `docker-compose exec api` if you would like with: `docker-compose exec api /bin/sh`  

1. Start Anchore
  > `docker-compose up -d`
2. Add Quay Registry
  > `anchore-cli registry add --insecure quay.agilesof.com QUAYUSER QUAYPASS`
3. Add Image to be analyzed
  > `docker-compose exec api anchore-cli --debug --insecure image add IMAGE`
4. Get Status
  > `docker-compose exec api anchore-cli image wait IMAGE`
5. Get list of vulnerabilities
  > `docker-compose exec api anchore-cli --json image vuln IMAGE VULNM_TYPE > 'OUT.json'`  

**VULN_TYPE**: os, on-os, all


# Notes

Because our quay uses a self signed certificate, you must use the `--insecure` flag when using anchore-cli

> [Follow Up](https://opensource.com/article/18/8/tools-container-security)
> [Tutorial](https://geekflare.com/anchore-container-security-scanner/)