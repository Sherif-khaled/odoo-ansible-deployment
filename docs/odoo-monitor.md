# Odoo Service Monitor & Auto-Healing Script

This project contains a **professional bash script** to **monitor**, **auto-heal**, and **notify** about the Odoo service status.

If the Odoo service stops because of **OOM (Out of Memory Kill)** or **Missing Python Module**, the script will:
- Automatically fix the problem.
- Restart the service.
- Send a detailed notification to a Slack channel.

---

## 📋 Features

- 🔥 Detect if Odoo service is killed by OOM and auto-restart it.
- 📦 Detect missing Python modules, auto-install them, and restart the service.
- 🚀 Send detailed Slack notifications with:
  - CPU usage.
  - Memory usage.
  - Reason of stopping.
  - Current status.
- 🛡️ Self-healing system with smart monitoring loop.
- 📑 Full logging to `/var/log/odoo/odoo_monitor.log`.
- 🔁 Dynamic checking (faster after failure, slower when normal).

---

## ⚙️ How to Use

### 1. Prepare the Bash Script

Start the odoo monitor service:

```bash
systemctl start odoo-monitor
```

Start the odoo monitor service:

```bash
systemctl status odoo-monitor
```