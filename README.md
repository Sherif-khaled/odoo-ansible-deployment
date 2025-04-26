# Odoo Ansible Deployment

A complete **Ansible-based automation toolkit** for deploying Odoo on either **cloud servers** or **localhost** environments. This project simplifies the setup and maintenance of Odoo instances and integrates with external tools such as **Cloudflare** and **GitHub/GitLab** for domain management.

## 🌟 Features

- ✅ **Supports Odoo versions 15, 16, 17, 18**
- ☁️ **Deploy to cloud or local environments**
- 🌐 **Link a custom domain to your deployment**
- 🔒 **Enable and configure SSL certificates**
- 🛡️ **Auto-create DNS records in Cloudflare (if using Cloudflare as DNS editor)**
- 🧩 **Auto-create GitHub repo if using an organization**
- 🛠️ **Auto-initialize the Odoo database**
- 🚀 **Deploy odoo helper scripts**

## 📁 Project Structure

```sh
└── odoo-ansible-deployment/
    ├── README.md
    ├── ansible.cfg
    ├── group_vars
    │   └── all.yml
    ├── inventory
    │   └── hosts.ini
    ├── mplay.yml
    └── roles
        ├── cloudflare
        │   └── tasks
        │       └── main.yml
        ├── common
        │   ├── handlers
        │   │   └── main.yml
        │   └── tasks
        │       └── main.yml
        ├── deploy_scripts
        │   ├── handlers
        │   │   └── main.yml
        │   |── tasks
        │   │    └── main.yml
        |   └── templates
        │       └── database_backup.sh.j2
        |       └── odoo_monitor.sh.j2
        |       └── odoo-monitor.service.j2
        ├── github
        │   ├── handlers
        │   │   └── main.yml
        │   └── tasks
        │       └── main.yml
        ├── nginx
        │   ├── handlers
        │   │   └── main.yml
        │   ├── tasks
        │   │   └── main.yml
        │   └── templates
        │       └── nginx.conf.j2
        ├── odoo
        │   ├── handlers
        │   │   └── main.yml
        │   ├── tasks
        │   │   └── main.yml
        │   └── templates
        │       ├── odoo.conf.j2
        │       └── odoo.service.j2
        ├── odoo_database
        │   └── tasks
        │       └── main.yml
        └── postgersql
            ├── handlers
            │   └── main.yml
            └── tasks
                └── main.yml
```

🚀 Getting Started
1. Clone the Repository
``` bash
❯ git clone https://github.com/sherifkhaled2022/odoo-ansible-deployment.git
❯ cd odoo-ansible-deployment
```

2. Configure Inventory
Edit the ansible/inventory/hosts file with your target server IP and access credentials.

3. Define Variables
Update ``` ansible/vars/main.yml ``` to include:

This tables provides a comprehensive explanation of all variables used in the Odoo installation and configuration process.

### Odoo Configuration

| Variable | Description | Default | Options |
|----------|-------------|---------|---------|
| `odoo_version` | The version of Odoo to install | `17` | Any supported Odoo version number |
| `odoo_home_path` | The installation directory for Odoo | `/opt/odoo{{ odoo_version }}` | Any valid path |
| `odoo_install_type` | Determines whether to install Community or Enterprise edition | `enterprise` | `community`, `enterprise` |
| `odoo_user` | The system user that will run the Odoo service | `odoo{{ odoo_version }}` | Any valid username |
| `odoo_port` | The port on which Odoo will listen | `8077` | Any valid port number |

### PostgreSQL Configuration

| Variable | Description | Default | Options |
|----------|-------------|---------|---------|
| `postgres_host` | The hostname of the PostgreSQL server | `localhost` | Any valid hostname or IP address |
| `postgres_port` | The port on which PostgreSQL listens | `5432` | Any valid port number |
| `postgres_user` | The PostgreSQL user for Odoo | `odoo{{ odoo_version }}` | Any valid PostgreSQL username |
| `postgres_password` | The PostgreSQL user password | `odoo` | Any valid password string |

### Odoo Security

| Variable | Description | Default | Options |
|----------|-------------|---------|---------|
| `odoo_master_password` | The master password for Odoo's database manager | (Empty string) | Any secure password string |

### Database Creation

| Variable | Description | Default | Options |
|----------|-------------|---------|---------|
| `create_database` | Whether to create an initial Odoo database | `true` | `true`, `false` |
| `odoo_db_name` | The name for the initial Odoo database | `production` | Any valid database name |
| `odoo_admin_password` | The admin user password for the created database |(Empty string) | Any secure password string |
| `odoo_username` | The admin username for the created database | `admin` | Any valid username |
| `odoo_country_code` | The country code for initial localization | `US` | Any valid 2-letter country code |
| `odoo_lang` | The language code for initial localization | `en_US` | Any valid language code |
| `db_creation_timeout` | Timeout (in seconds) for database creation | `2000` | Any positive integer |

### Additional Software

| Variable | Description | Default | Options |
|----------|-------------|---------|---------|
| `install_wkhtmltopdf` | Whether to install wkhtmltopdf (required for PDF generation) | `yes` | `yes`, `no` |

### Nginx Configuration

| Variable | Description | Default | Options |
|----------|-------------|---------|---------|
| `install_nginx` | Whether to install and configure Nginx as a reverse proxy | `true` | `true`, `false` |
| `odoo_url` | The domain name that will be used to access Odoo | (Empty string)| Any valid domain name |
| `install_ssl` | Whether to install and configure SSL using Let's Encrypt | `true` | `true`, `false` |
| `certbot_email` | Email address for Let's Encrypt notifications | (Empty string) | Any valid email address |

### Odoo Enterprise Configuration

| Variable | Description | Default | Options |
|----------|-------------|---------|---------|
| `enterprise_user_token` | The access token for downloading Odoo Enterprise | (Empty string) | Valid Odoo Enterprise token |

### Cloudflare Integration

| Variable | Description | Default | Options |
|----------|-------------|---------|---------|
| `enable_create_cloudflare_record` | Whether to create DNS records in Cloudflare | `true` | `true`, `false` |
| `cloudflare_api_token` | API token for Cloudflare authentication | (Empty string) | Valid Cloudflare API token |


## GitHub Repository Integration

| Variable | Description | Default | Options |
|----------|-------------|---------|---------|
| `create_repo` | Whether to create a GitHub repository for this installation | `true` | `true`, `false` |
| `github_repo` | The name of the GitHub repository to create | (Empty string) | Any valid repository name |
| `ssh_key_name` | The name of the SSH key for GitHub authentication | `{{ github_repo }}` | Any valid key name |
| `github_org` | The GitHub organization where the repository will be created | (Empty string) | Any valid GitHub organization |
| `repo_private` | Whether the created repository should be private | `true` | `true`, `false` |
| `github_token` | GitHub personal access token for repository creation | (Empty string) | Valid GitHub PAT |

## Deploy Scripts

| Variable | Description | Default | Options |
|----------|-------------|---------|---------|
| `deploy_scripts` | Whether to deploy odoo scripts | `true` | `true`, `false` |
| `database_backup_script` |  Deploy Database backup script | `true` | `true`, `false` |
| `database_backup_path` | Default database backup directory path | `{{ odoo_home_path }}/database_backup` | Any directory path name |
| `odoo_monitor_script` | The GitHub organiDeploy odoo monitor and self-healing script | `true` | `true`, `false` |


## Slack Notifications

| Variable | Description | Default | Options |
|----------|-------------|---------|---------|
| `send_to_slack` | Whether to send installation notifications to Slack | `true` | `true`, `false` |
| `slack_webhook_url` | The webhook URL for Slack integration | (Empty string) | Valid Slack webhook URL |
| `slack_channel` | The Slack channel to send notifications to | `#ansible-notifications` | Any valid Slack channel |
| `slack_username` | The username to display for Slack notifications | `Odoo Installer Bot` | Any string |

## Usage Notes

1. **Security Considerations:**
   - Ensure `odoo_master_password`, `odoo_admin_password`, `postgres_password`, and API tokens are strong and secure
   - Consider using Ansible Vault to encrypt sensitive values

2. **Domain Configuration:**
   - When setting `odoo_url`, make sure the domain is properly registered and pointing to your server
   - If using Cloudflare, the DNS records will be created automatically if `enable_create_cloudflare_record` is true

3. **Enterprise Installation:**
   - For enterprise installations, you must provide a valid `enterprise_user_token`
   - This token can be obtained from the Odoo Enterprise portal

4. **Database Creation:**
   - Set `create_database` to false if you prefer to create the database manually
   - When true, a database named by `odoo_db_name` will be created with an admin user

4. Run the Playbook
```bash
❯ ansible-playbook -i ansible/inventory/hosts ansible/playbooks/deploy.yml
```

#### ☁️ Cloudflare Integration
If you manage your DNS via Cloudflare, the playbook can auto-create A records for your domain.

#### 🧩 Auto GitHub Repo Creation ####
If you’re using GitHub organizations, the playbook can auto-create a new repository for your Odoo deployment. Great for automation and managing multiple clients with separate repos.

#### 🔐 SSL Integration
Certbot is used to issue and install SSL certificates for your domain automatically.

#### 🚀 Deploy Odoo Scrits
 - Deploy odoo backup database script to take database backup daily at 3A.M and store the backup in ~/database_backup directoey
 - [Deploy odoo monitor and self-healing script](docs/odoo-monitor.md)


## 📚 Long Description
This project is designed to eliminate the complexity of deploying Odoo across various environments using Ansible, ensuring that you can focus on development and business logic rather than infrastructure management. It supports multiple Odoo versions (15 through 18) and is built with a focus on scalability, automation, and ease of use.

Whether you're a developer, system admin, or DevOps engineer, this toolkit enables you to:

* Deploy Odoo in minutes

* Link your app to a domain and secure it with SSL

* Automatically integrate with Cloudflare DNS if used

* Initialize and migrate Odoo databases without manual steps

* Keep your instance up-to-date with auto Git pull scripts

This makes it ideal for businesses or freelancers running Odoo for multiple clients, or managing multi-instance environments across production and staging.

## ✅ Requirements
* Ansible 2.10+

* A Linux server (Ubuntu/Debian recommended)
* Python 3
* Git
* Cloudflare API Token (optional)
* (Optional) GitHub Personal Access Token (with `repo` & `admin:org` scopes)


## 🤝 Contributing
Contributions are welcome! Feel free to open issues or submit pull requests for improvements, bug fixes, or new features.

## 📜 License
This project is licensed under the MIT License.


