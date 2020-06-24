# Anchore Setup

---

## Installation

1. Install Anchore CLI

This is done via PIP: `pip install --user --upgrade anchorecli`

  * Make sure that the command `anchore-cli` is in your PATH, if not locate it:
> `find ~ -name anchore-cli`
  * Add this file (or parent directory to shell profile)

1. Clone this repo
2. Tune config.yaml settings to your liking
3. Bring up Anchore containers
> `docker-compose up -d`

## Usage

1. For basic scans, you need to first add the image to be analyzed:
> `anchore-cli --u admin --p foobar image add quay.agilesof.com/brandi-dev/ad-hoc-service:latest`

2. Unfortanately Anchore does not inform you once an image is scanned, you can view an individual image status with:
> `anchore-cli --u admin --p foobar image get quay.agilesof.com/brandi-dev/ad-hoc-service:latest`

3. Viewing Security Vulnerabilities
The basic format is: `anchore-cli image vuln INPUT_IMAGE VULN_TYPE`
  * VULN_TYPE can either be `os`, `non-os` or `all`

4. Output to JSON
> `anchore-cli --u admin --p foobar --json image vuln quay.agilesof.com/brandi-dev/ad-hoc-service:latest all > 'cve.json'`