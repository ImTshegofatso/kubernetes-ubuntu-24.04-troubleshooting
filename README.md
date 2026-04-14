# Kubernetes on Ubuntu 24.04 – Troubleshooting Case Study (Snap-Based)

This repository documents the setup of a single-node Kubernetes cluster on **Ubuntu 24.04** with a strong focus on **troubleshooting, root-cause analysis, and decision-making**, rather than a simple “happy-path” installation.

The content reflects real challenges encountered during installation and explains how those challenges were identified and resolved.

---

## Environment

- Operating System: Ubuntu 24.04 LTS  
- Virtualization: Oracle VirtualBox  
- Cluster Type: Single-node (control plane and worker on the same machine)  
- Kubernetes Installation Method: Snap  
- Container Runtime: containerd  

---

## Objective

The objective was to install Kubernetes using `kubeadm`, `kubelet`, and `kubectl` for learning and lab purposes.  
During the process, multiple issues prevented a straightforward installation and required troubleshooting across networking, operating system defaults, and container runtime configuration.

---

## Issues Encountered

### APT-Based Installation Limitations
Kubernetes packages are not included in Ubuntu’s default repositories. Installing via APT requires external repositories, GPG key management, and reliable DNS resolution. Repository configuration repeatedly failed, blocking installation.

### DNS Resolution Problems
Commands such as `curl` and `apt` failed with errors indicating that hostnames could not be resolved. Investigation showed that DNS was provided by VirtualBox NAT, but external domains required for Kubernetes installation were not resolving correctly.

### kubelet Startup Failures
After installation attempts, the kubelet service failed to stabilise. Further investigation revealed a mismatch between Kubernetes expectations and Ubuntu 24.04 defaults, specifically related to cgroup v2 and container runtime configuration.

---

## Troubleshooting Approach

A structured troubleshooting approach was followed:
- Network connectivity and DNS behaviour were tested using `ping`, `curl`, and `resolvectl`.
- Errors were analysed to distinguish environmental issues from configuration mistakes.
- To reduce dependency on external repositories and DNS reliability, the installation strategy was changed from APT to **Snap**.
- The container runtime configuration was updated to align with Ubuntu 24.04 defaults by enabling systemd-based cgroup management.

---

## Final Solution

### Kubernetes Installation Using Snap

```bash
sudo snap install kubectl --classic
sudo snap install kubeadm --classic
sudo snap install kubelet --classic
