# **Building Security into Scent: DesignMyFragrance E-Commerce Security Write-up**

“Fragrance is the unseen, unforgettable accessory.” — Coco Chanel  
But what if the most unforgettable part of a fragrance website is how securely it's built?

In my recent project for E-Commerce Security, I built DesignMyFragrance — a luxury perfume e-commerce platform. While most would focus on branding and visuals (which I did too), my primary goal was making sure the website could handle real-world threats — from brute-force login attempts to SSL misconfigurations — while still delivering a seamless user experience.

The Mission
The objective was clear:  
Build a fully functional e-commerce site on WordPress that’s not only sleek and fast but also resilient to cyber threats.
I chose WordPress for rapid deployment, and WooCommerce for the backbone of my shopping experience. Security was layered in from the get-go — I wasn’t going to let bad actors crash this fragrance party.

Foundation: Picking the Right Stack
I went with the Twenty Twenty-Four theme — modern, minimal, and perfect for showcasing high-resolution product images.
Here’s the plugin stack I rolled with:
- WooCommerce — The core shop engine  
- WooPayments (Stripe) — Secure, PCI-DSS compliant payments  
- UpdraftPlus — Scheduled backups in case things go south  
- WP 2FA — Multi-factor authentication for account hardening  
- WooCommerce Shipping & Tax — To automate logistics and compliance

Hardening the Platform
I didn’t just slap a plugin on and call it “secure.” Each decision came with a specific threat model in mind:
- Brute-force attacks? Handled via WP 2FA + custom admin URL.
- SSL/TLS issues? Fixed with Apache config tuning.
- Unauthorized access? Limited SSH and WordPress admin access with AWS Security Groups (GWU IPs only).
- Payment fraud? Stripe’s fraud detection + WooPayments’ heuristics.
- **Data loss**? Scheduled backups with UpdraftPlus for peace of mind.

All data in transit was encrypted with **HTTPS**, and the site architecture was tested across browsers and devices to ensure **responsiveness**.

Behind the Scenes: Real Debugging Moments

No project is complete without its fair share of *"Oh no..."* moments.
Issue 1:MySQL wouldn’t connect. Turns out, my EC2 instance hadn’t properly synced DB configs — a simple `bind-address` tweak fixed it.  
Issue 2:SSL setup broke the site. I had misconfigured the Apache virtual host block, missing a redirect and CA bundle. After some digging, `openssl s_client` became my best friend.  
Issue 3:AWS Security Groups initially left the server exposed. Restricting to known IPs solved it.

Red Team Mindset: Security Threat Modeling

Here’s how I approached potential vulnerabilities:

|       Threat       |                Mitigation                  |
|-------             |------------                                |
| Brute-force Login  | MFA with WP 2FA + admin URL obfuscation    |
| XSS                | Input validation + Content Security Policy |
| Payment Fraud      | Stripe’s risk monitoring                   |
| DDoS               | AWS Shield + CloudFront CDN                |
| Malware/Backdoors  | Wordfence scans + GuardDuty monitoring     |

I also created a **Security Breach Response Plan** that includes isolating the site, analyzing GuardDuty logs, restoring from backups, and patching everything like a responsible sysadmin.

Lessons Learned

- Security is **not** an afterthought — it’s baked in from the beginning.
- SSL is easy to mess up, but once done right, it’s a foundational shield.
- AWS Security Groups are powerful but require precision.
- WooCommerce’s extensibility is both a blessing and a responsibility.

Final Thoughts

This project made me appreciate the delicate balance between usability, design, and cybersecurity. Just like fragrances combine top, middle, and base notes — e-commerce security requires layers: visual polish, robust features, and airtight defenses.
