---
layout: post
description: "Learn how to fix a stuck or ghost virtual machine in XCP-ng or XenServer when 'xe' shows it running but it won't shut down or destroy."
title: "🧟‍♂️ How to Fix a Stuck or Ghost VM in XCP-ng or XenServer"
author: Bikarna Pokharel
categories: [xcp-ng, virtualization, devops]
permalink: /xcp-ng/virtualization/devops/2025/05/09/ghost-vm-fix.html
tags: [xenserver, xcp-ng, ghost-vm, vm-reset, xe, virtualization]
date: 2025-05-09
---

> A practical guide to resolving a VM that appears to be running, but isn't — and refuses to shut down or be destroyed.
📅 **Posted on May 9, 2025 – Eastern European Summer Time (EEST)**
---

## 🧾 Symptoms

You might run into a VM that:

- Appears as **Running** in XCP-ng Center or `xe vm-list`
- **Does not appear** in `xl list`
- **Cannot be shut down** via `xe vm-shutdown` 
- **Cannot be destroyed**, with an error like:
You attempted an operation on a VM that was not in an appropriate power state.
expected: halted, suspended
actual: running


---

## ✅ The Fix

### Step 1: Confirm the Ghost State

```bash
xe vm-list uuid=<vm-uuid> params=name-label,power-state
xl list
```
If the VM appears in xe as running but does not appear in xl list, it is a ghost.

### Step 2: Reset the Power State
```bash
xe vm-reset-powerstate uuid=<vm-uuid> --force
```
✅ This forcibly tells XAPI that the VM is halted.

### Step 3: Destroy the VM (if needed)
```bash
xe vm-destroy uuid=<vm-uuid>
```
