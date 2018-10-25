# Platform Firmware Security for Enterprise Guidance Summary Checklist

A checklist summary of book text and references, particularly applicable NIST standards. When you pick up this list, you'll partway through the *Hardware Lifecycle* for all of your current hardware. We recommend you simply start implementation at the beginning (Policies and Procedures), and apply the entire checklist to your next naturally scheduled hardware purchase. Then, as time permits, proceed through the process for older hardware, starting with hardware in the most sensitive positions eg:

 * Officer, Finance and Legal laptops

 * Servers subject to *PCI*, *HIPAA*, etc

This may trigger some unscheduled purchases for security reasons, but the older hardware can be reused in less sensitive applications.

---

## Meta: Policies and Procedures

[] Review and update existing corporate security policies to include firmware security.

[] Review and update existing corporate security procedures to include firmware security.

## Pre-Acquisition Research Phase

Include hardware and firmware in threat modeling. Think broadly about firmware. A computer includes multiple firmware images: microcode, multiple platform firmware images (UEFI or BIOS, ACPI, etc.), firmware on each adapter card, firmware on each peripheral and its adapter cables.  Besides 'bare-metal' firmware, virtualization technologies have virtual firmware drivers that can also be attacked; virtual firmware has it's own set of attacks, some shared with bare-metal firmware, some unique to the VMM implementation, some shared with traditional app-level attacks.  In addition to firmware 'blobs' stored in flash and NVRAM, UEFI stores firmware as files on the FAT-based disk partition, EFI System Partition (ESP), potentially editable by attackers on the ACL-less FAT volume.

Plan to purchase secure and securable systems and to secure the entire system at all levels: hardware, firmware, and software. Read entire guidance document and plan for implementation.

Educate technical staff to the firmware threat. Secure systems for non-technical users, and train them about evil maid attacks - particularly with mobile compute devices.

Add firmware to Hardware Lifecycle: System Administrators should integrate NIST 147, which provides Secure Firmware Lifecycle, to augment existing enterprise hardware lifecycle.  Digital Forensic and Incident Response (DFIR) teams should expand their checkslists to include contacts with firmware vendors, OEMs, IBVs, NIST 147 is a starting point.

Ensure availability of, test and learn to use vendor specific firmware tools, including golden image hashes, and signed updates.

Update news and training sources for firmware, including general bug fixes, and critical security updates. Ensure that coverage and updates are fit to purpose.

Request security data from vendors. Source this data yourself eg: acquire a single example of a new model and ensure it passes CHIPSEC security tests.

Set company policy:
 * For security features that must be available in newly-acquired systems. eg: Secure Boot, Verified Boot, TPMv2, CHIPSEC tests.
 * Against grey market hardware.
 * For security features that must be configured in new systems.

Consider currently deployed systems. Are they fit to purpose? Should some new hardware purchases be accelerated, and older systems redeployed to less sensitive applications?

---

## Provisioning Phase

Configure firmware security settings based on company policy. Eg, enable/disable VT/TPM/TXT, WakeOn<x> features, firmware/BIOS password, firmware network ability, Secure Boot, etc.

Ensure all vendor firmware is up-to-date, with up-to-date keys for any signed code.

Register endpoint identity and BIOS integrity information in system inventory

During initial system acquisition, make your own initial 'golden image' of the platform firmware, save the file for future image comparisions, as initial baseline.  If vendor provides their own golden image, compare your image with their image, or hash.

Use built-in hardware/firmware security features: Use existing vendor firmware security features, TPM, UEFI Secure Boot, Verified Boot, Measured Boot, Trusted Boot, Heads, etc. 

Use signed firmware images: For UEFI, along with Secure Boot, only use firmware that has been properly signed.  Understand the code signing process and where gaps may exist -- eg, some certs are given to closed-source binaries, no source code audit permitted.

---

## Operations and Maintenance Phase

Keep firmware up-to-date: In addition to OS and app software, also update the firmware, ideally via signed, automated mechanisms such as Windows Update, Linux *fwupd (LVFS)*.

Apply latest Secure Boot blacklisted key database from UEFI.org and Microsoft.com.

Verify firmware updates with hashes.

Perform periodic firmware security scans. Download firwmare blobs and check for unexpected changes with hashes. 

Consider "IT hands on" interactions with end user equipment an opportunity to run automated firmware-level security tests.

Monitor all network traffic from/to firmware. 

Audit OS-level code which instigates firmware updates. If hardening OS/applications, consider restricting access to tools that perform firmware updates to authorized accounts.

During a security incident, use traditional malware analysis techniques to look for firmware-level malware, and add firmware specific tools such as UEFITool, ACPItool.

During a security incident, the response should include re-obtaining firmware images, and re-running firmware security tests (eg, CHIPSEC), to compare with previous images/results.

Watch vendor security advisory sites of all vendors in supply-chain, for security advisories, and new detection tools to use. Main CPU, TPM vendor, OS vendor, OEM, all IHVs, etc.

---

### Recovery (Incident Response) Phase 

Use forensic tools to compare current rom image to previous scans for changes.

Re-run security tests, compare to last-known-good results, look for signs of an attacker.  For example, SPI protections were disabled, but now are enabled.

If system is compromised with OS/app-level malware, it may have updated firmware.  Validate the entire system, firmware and OS/level tests.  Restore a system using both firmware and OS 'golden images'.

After a security incident, Incident Response will need additional time to cleanse a system of firmware-level malware. In some cases, the system may be bricked and not recoverable. 

After a security incident, the IR team postmortem analysis should include which vendors have inadequate tools/images for firmware recovery, and discard those systems, or redeploy them to less important roles.

---

## Disposition Phase

Restore firmware to factory reset before disposition. Before disposing of a system, in addition to sanitizing disk media, reset the firmware to a known-good state, using golden image from vendor.

Sanitize any PII in firmware before disposition. Before disposing of a system, ensure that any PII stored in firmware, user authentication data, TPM secrets, etc. are reset. Ask vendor how to restore any PII stored by firmware.
