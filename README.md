# Grafana Installation and Dashboard Creation for Ubuntu Server Performance

## **Objective**
This report outlines the steps taken to install Grafana on an Ubuntu server, configure it to collect performance metrics using Azure Monitor, and create a Grafana dashboard to visualize these metrics.

## **Prerequisites**
- An Ubuntu server (version 18.04 or later)
- An Azure account with permissions to create and manage resources
- Basic understanding of the Linux command-line interface

## **Lab Tasks**

### **Task 1: Prepare Your Ubuntu Server**
1. Update and upgrade system packages:
   ```bash
   sudo apt-get update && sudo apt-get upgrade -y
   ```
2. Install required dependencies:
   ```bash
   sudo apt-get install -y apt-transport-https software-properties-common wget
   ```

### **Task 2: Install Grafana**
1. Add Grafana’s official APT repository:
   ```bash
   sudo mkdir -p /etc/apt/keyrings/
   wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
   echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
   ```
2. Install Grafana:
   ```bash
   sudo apt-get update
   sudo apt-get install grafana
   ```
3. Start and enable Grafana:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl start grafana-server
   sudo systemctl enable grafana-server
   ```
4. Verify Grafana service status:
   ```bash
   sudo systemctl status grafana-server
   ```
5. Enable port 3000 in the firewall:
   ```bash
   sudo ufw allow 3000/tcp
   ```

### **Task 3: Connect Grafana to Azure Monitor**
1. Enable Managed Identity in the Grafana VM.
2. Assign the following roles to the VM's managed identity in Azure:
   - **Monitoring Reader Role** on Azure Monitor
   - **Reader Role** on the Azure Subscription
3. Configure Managed Identity authentication in Grafana by editing the configuration file:
   ```bash
   sudo nano /etc/grafana/grafana.ini
   ```
   - Modify settings to enable managed identity authentication for data sources.
4. Restart Grafana server:
   ```bash
   sudo systemctl stop grafana-server
   sudo systemctl start grafana-server
   ```
5. Open Grafana in a web browser using:
   ```
   http://<your-server-ip>:3000
   ```
   - Default login: **admin/admin**
6. Navigate to **Configuration > Data Sources** and add **Azure Monitor** as a data source.
7. Select **Managed Identity Authentication** and test the connection.

### **Task 4: Create a Dashboard in Grafana**
1. Click the **+** icon and select **Dashboard**.
2. Click **Add new panel**.
3. Select **Azure Monitor** as the data source.
4. Choose relevant metrics (e.g., CPU usage, memory usage, network I/O).
5. Customize the panel with:
   - Thresholds
   - Colors
   - Labels
6. Save the panel and dashboard.

## **Conclusion**
This lab successfully demonstrated the installation of Grafana, connection to Azure Monitor, and the creation of a performance monitoring dashboard for an Ubuntu server. The setup provides real-time insights into system performance, aiding in proactive monitoring and optimization.
