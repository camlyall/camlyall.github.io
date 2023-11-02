---
layout: default
title: EC2 Notes
parent: AWS
---

# EC2 Notes
{: .no_toc }

Last Modified: {% last_modified_at %}

<details open markdown="block">
  <summary>
   Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

{: .no_toc .text-delta }

---

## Documentation
* [https://aws.amazon.com/ec2/getting-started/](https://aws.amazon.com/ec2/getting-started/)

---

## User Data
Must include shebang

### Verify User Data
* [https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-add-user-data.html](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-add-user-data.html)

Every instance can use the following URI to retrieve user data from within a running instance.
```bash
curl http://169.254.169.254/latest/user-data
```


## Load Balancer
## Stickiness
Enable stickiness to maintain users accessing the same instance.
Can set duration, example: 1 day
