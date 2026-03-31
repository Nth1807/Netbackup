# ⬡ NetBackup — Network Device Config Backup Tool
<img width="931" height="425" alt="image" src="https://github.com/user-attachments/assets/15920096-79e3-4c4c-90c5-abb564559755" />

# ⬡ NetBackup — Network Device Config Backup Tool

A web application for backing up configurations of Switches / Routers / Firewalls via SSH, with automatic push to GitHub.

---

## 🚀 Installation & Run

```bash
# 1. Clone / copy this folder 
cd netbackup

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run server
python app.py
# → http://localhost:5000
```

---

## 📁 Project Structure

```
netbackup/
├── app.py              ← Flask backend (SSH + Git + REST API)
├── requirements.txt
├── config.json         ← Auto-generated khi save settings
├── backups/            ← File .txt backup local save
├── git_work/           ← Temp folder clone GitHub repo
└── static/
    └── index.html      ← Dashboard UI
```

---

## ⚙️ GitHub Configuration

1. Go to **Settings** in the dashboard
2. Fill in:
   - **GitHub Token**: Personal Access Token (requires `repo`)
     → https://github.com/settings/tokens/new
   - **Repository**: `username/name-repo` (The repository must already exist)
   - **Branch**: `main` or another branch 
3. Click **Save Settings**

---

## 🖥️ Add Device

Go to **Devices** → **Add Device**, Fill in:

| Field       | Example              |
|-------------|--------------------|
| Name        | Core-SW-01         |
| Host/IP     | 192.168.1.1        |
| Port        | 22                 |
| Username    | admin              |
| Password    | (Or SSH key path)|
| Type        | cisco_ios, juniper, fortigate... |

---

## 📋 Supported Device Types 

| Type         | Commands Run                                     |
|--------------|----------------------------------------------------|
| `cisco_ios`  | show running-config, show version, show ip int br  |
| `cisco_nxos` | show running-config, show version, show int brief  |
| `juniper`    | show configuration, show version, show interfaces  |
| `fortigate`  | show full-configuration, get system status         |
| `mikrotik`   | export verbose, /system resource print             |
| `generic`    | show running-config                                |

---

## 🔄 Backup

- **Dashboard** → **Run Backup All** to backup all devices
- Or click **▶ Backup** on each device
- File are saved in `backups/` with the format: `DeviceName_YYYYMMDD_HHMMSS.txt`
- After backup, files are automatically pushed to github ( if configured)

---

## 🌐 API Endpoints

| Method | Path                    | Description               |
|--------|-------------------------|---------------------------|
| GET    | /api/stats              | Dashboard stats           |
| GET    | /api/devices            | List devices              |
| POST   | /api/devices            | add new device            |
| PUT    | /api/devices/:id        | Update device             |
| DELETE | /api/devices/:id        | Delete device             |
| POST   | /api/backup             | Start backup job          |
| GET    | /api/jobs               | jobs history              |
| GET    | /api/jobs/:id           | jobs detail               |
| GET    | /api/backups            | list backup files         |
| GET    | /api/backups/:file      | Download file backup      |
| GET    | /api/config             | view config               |
| POST   | /api/config             | Update config             |

---

## 🔐Security (Production)

- run behind `nginx` with auth (basic auth or VPN only)
- use SSH key instead of password
- keep `config.json` (contains token) private
- Set `FLASK_ENV=production`
