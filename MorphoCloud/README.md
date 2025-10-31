# MorphoCloud: On-Demand Cloud for 3D Slicer & SlicerMorph


<p align="center">
  <img src="https://raw.githubusercontent.com/MorphoCloud/MorphoCloudInstances/main/MC_Logo.png" alt="MorphoCloud Logo" width="280">
</p>

**What is MorphoCloud:** MorphoCloud provides **on-demand cloud instances** to support computational morphology, 3D morphometrics, and
biomedical imaging research. It allows researchers and educators to launch powerful
[JetStream2](https://jetstream-cloud.org/) virtual machines — preloaded with
[3D Slicer](https://download.slicer.org) and
[SlicerMorph](https://SlicerMorph.org) — by simply opening and commenting on
GitHub issues.

## ✨ What You Can Do with MorphoCloud

- **Run Slicer in your browser**: GPU-enabled remote desktops with up to 40GB
  GPUs and 1TB RAM.
- **Request and manage instances with GitHub Issues**: no portals, no manual
  provisioning.
- **Automated lifecycle management**: Create, shelve, unshelve, renew, or delete
  instances via simple `/commands`.
- **Support research and teaching**: Ideal for occasional high-performance
  computing needs in morphology and imaging. Support for short-courses and workshops.
  
## 🖥️ Available Instance Types

  | Flavor | RAM    | Cores | GPU         | Storage |
  | ------ | ------ | ----- | ----------- | ------- |
  | g3.l\* | 60GB   | 16    | A100 (20GB) | 100GB   |
  | g3.xl  | 125GB  | 32    | A100 (40GB) | 100GB   |
  | m3.x   | 250GB  | 64    | None        | 100GB   |
  | r3.l   | 500GB  | 64    | None        | 100GB   |
  | r3.xl  | 1000GB | 128   | None        | 100GB   |

  \*`g3.l` is the default instance.<br>
  [Check real-time JetStream2 resource availability →](https://docs.jetstream-cloud.org/overview/status/#availability-of-scarce-resources)

## 🔑 Commands

  Once approved, you can control your instance from the issue itself:

  | Command            | Description                 |
  | ------------------ | --------------------------- |
  | `/create`          | Provision a new instance (typically done only once)   |
  | `/shelve`          | Suspend (turn off) instance |
  | `/unshelve`        | Resume a shelved instance     |
  | `/delete_instance` | Delete an instance          |
  | `/delete_volume`   | Remove associated storage   |
  | `/renew`           | Extend lifespan of the instance for another 60 days  |
  | `/email`           | Resend access email (available only when the instance is online)        |
  
  
## 🚀 Quick Start

  - **For Researchers / Educators**: 👉 Request an instance via
    [MorphoCloudInstances issue templates](https://github.com/MorphoCloud/MorphoCloudInstances/issues/new/choose).
    Approval typically takes <24h.
	
  - **Important Usage note**: after the approval is granted, you can create your instance by entering the `/create` command on your GH issue page. When the instance becomes online you will receive an email from the MorphoCloudPortal that will provide the access credentails (see below). After becoming online an instance stays online only for 4 hours (unless you choose to extend your session, by clicking the icon on the desktop). After 4 hours, the instance shelves (suspends) itself. The next time you want to use it, you don't need to create the instance, but simply type `/unshelve` on your issue page to make it available online again. Each time your instance becomes online, MorphoCloudPortal will send you a new email with access credential, as those may change from time to time. 



## 🖼️ Desktop Environment

MorphoCloud instances provide graphical user interface (GUI) as well as command line interface (ssh). 

  <p align="center">
    <img src="https://raw.githubusercontent.com/MorphoCloud/MorphoCloudInstances/main/MCI_Desktop.png" width="650">
  </p>

  - **Side toolbar** (Ctrl/Cmd + Alt + Shift) for file transfers & clipboard.
  - **Shortcuts** for Slicer, SlicerMorph, and MyData storage.
  - **Right-click menu** to adjust resolution and display settings.
  - **Extend session** with one click (+4 hours).

  
You have two options to connect to GUI:

1. **Connect via web browser by clicking the Web connect link in the email**. This interface is called **Guacamole** and provides additional conveniences such as the side bar that allows for file transfers and clipboard functionality. The downside of the guacamole interface is that is doesn't handle the font and display scaling very well, and the clipboard functionality is cumbersome to use. 

  
2. **You can also use the TurboVNC** application, which you can download and install for your computer from https://github.com/TurboVNC/turbovnc/releases/tag/3.2.1 (Expand the Assets tab, and find the correct package for your computer). The benefit of TurboVNC is that it provides a much more realistic desktop experience with improved image quality and scaling, as well as copy/paste buffer that works natively. However, you cannot do file transfers from the TurboVNC connection. The access address for TurboVNC connection is also provided in the credentials email sent to you. The passphrase is the same whether you use the web browser (guacamole interface) or TurbovVNC.

You probably want to use both access methods in tandem (e.g., use Guacamole for easy file transfer to and from), and TurboVNC for nice visualizations. 

## ❗ Important Things to Consider ❗
1. Getting started on MorphoCloud is a two-step procedure. First, you make the request and get approved, and then you need to create your instance to start using MorphoCloud
2. You cannot change instance types after you created the issue and it is approved. If you need to switch to a different instance type, you will need to open a new issue.
3. Whether an instance can be created, or a created instance can become online depends on the resource availability on the JetStream at the time of the command. Because JetStream can be heavily used at times, we advise checking the availability of a specific instance type before creating or unshelving an instance. https://docs.jetstream-cloud.org/overview/status/#availability-of-scarce-resources.
4. On GPU instances (g3.l and g3.xl) the interactivity and performance of 3D Slicer greatly diminishes (almost to the point of not being able to use the instance), when you **volume render a 3D volume with any dimension equal to (or larger than) 4096 voxels**. If you have to work with such large volumes, consider other instance types. 


 