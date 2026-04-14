# Kubernetes on Ubuntu 24.04 – Troubleshooting Case Study (Snap-Based)

This repository documents the setup of a single-node Kubernetes cluster on **Ubuntu 24.04** with a strong emphasis on **troubleshooting, diagnosis, and decision-making** rather than a simple “happy‑path” installation.

The work presented here reflects real issues encountered during installation and the reasoning behind choosing alternative approaches to complete the setup successfully.

---

## Environment

- Operating System: Ubuntu 24.04 LTS  
- Virtualization: Oracle VirtualBox  
- Cluster Type: Single-node (control plane and worker on the same machine)  
- Kubernetes Installation Method: Snap  
- Container Runtime: containerd  

---

## Objective

The objective was to install Kubernetes using `kubeadm`, `kubelet`, and `kubectl` for learning and lab purposes. During the process, several issues were encountered that prevented a straightforward installation, requiring investigation and troubleshooting across multiple layers of the system.

---

## Issues Encountered

### APT-Based Installation Limitations
Kubernetes packages are not included in Ubuntu’s default repositories. Installing via APT requires external repositories, GPG keys, and reliable DNS resolution. Repository configuration repeatedly failed, preventing Kubernetes components from being installed successfully.

### DNS Resolution Problems
Commands such as `curl` and `apt` failed with errors indicating that hostnames could not be resolved. Investigation using system tools showed that DNS was being supplied by VirtualBox NAT, but external domains such as Kubernetes package hosts were not resolving correctly.

### kubelet Startup Failures
After partial installation attempts, the kubelet service failed to stabilise. Further investigation revealed a mismatch between Kubernetes expectations and Ubuntu 24.04 defaults, specifically related to cgroup v2 and container runtime configuration.

---

## Troubleshooting Approach

Each issue was investigated methodically:
- Network connectivity and name resolution were tested using tools such as `ping`, `curl`, and `resolvectl`.
- Failures were identified as environmental rather than command or syntax errors.
- To reduce dependency on external repositories and DNS reliability, the installation approach was changed from APT to **Snap**.
- The container runtime configuration was aligned with Ubuntu 24.04 defaults by enabling systemd-based cgroup management.

---

## Final Solution

### Kubernetes Installation Using Snap
Snap was used to install Kubernetes components in a self-contained and reliable manner:

```bash
snap install kubectl --classic
snap install kubeadm --classic
snap install kubelet --classic
