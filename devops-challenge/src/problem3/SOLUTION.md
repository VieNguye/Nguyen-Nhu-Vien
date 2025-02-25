Provide your solution here:

## **Troubleshooting High Memory Usage on Ubuntu 24.04 VM Running NGINX**

### **Step 1: Confirm the Issue**

1. **Check Memory Usage**  
   ```bash
   free -h
   ```
   - Verify if memory usage is indeed at **99%**.

2. **Analyze Running Processes**  
   ```bash
   top
   ```
   or  
   ```bash
   htop
   ```
   - Identify which processes are consuming the most memory.

3. **Inspect NGINX Memory Usage**  
   ```bash
   ps aux --sort=-%mem | head -10
   ```
   - Check if **NGINX** or another process is consuming excessive memory.

4. **Check Swap Usage**  
   ```bash
   swapon --summary
   ```
   - If swap is heavily used, the system might be running out of RAM.

---

## **Possible Causes & Resolutions**

### **Root Cause 1: NGINX Memory Leak or High Worker Processes**

#### **Scenario**  
- NGINX may have a misconfiguration leading to excessive memory usage.
- The worker processes might be **too high** for available system resources.

#### **Impact**  
- Increased response times and potential **downtime**.
- VM could become **unresponsive** due to memory exhaustion.

#### **Troubleshooting & Recovery**

1. **Check Worker Processes in NGINX Config**  
   ```bash
   cat /etc/nginx/nginx.conf | grep worker_processes
   ```
   - If set too high, it can cause **excessive memory usage**.

2. **Adjust Worker Processes Dynamically**
   - Set the worker processes to **auto**:
     ```bash
     sudo nano /etc/nginx/nginx.conf
     worker_processes auto;
     ```

3. **Restart NGINX**  
   ```bash
   sudo systemctl restart nginx
   ```

4. **Monitor Memory Usage Again**  
   ```bash
   free -h
   top
   ```

5. **Check NGINX Error Logs for Memory Issues**  
   ```bash
   sudo tail -f /var/log/nginx/error.log
   ```

---

### **Root Cause 2: High Traffic Overloading NGINX**

#### **Scenario**  
- A surge in incoming traffic could cause **NGINX to consume more memory**.
- The VM **doesn’t have enough resources** to handle the load.

#### **Impact**  
- Users may experience **slowness or downtime**.
- If not mitigated, the system may **crash**.

#### **Troubleshooting & Recovery**

1. **Check Active Connections**  
   ```bash
   sudo netstat -anp | grep nginx | wc -l
   ```
   - If unusually high, this confirms a traffic spike.

2. **Analyze NGINX Access Logs for Spikes**  
   ```bash
   sudo tail -f /var/log/nginx/access.log
   ```

3. **Rate Limit Requests to Reduce Load**  
   - Add the following to the **nginx.conf**:
     ```nginx
     limit_req_zone $binary_remote_addr zone=one:10m rate=10r/s;
     ```
     Inside the server block:
     ```nginx
     location / {
         limit_req zone=one burst=20 nodelay;
     }
     ```

4. **Restart NGINX to Apply Changes**  
   ```bash
   sudo systemctl restart nginx
   ```

5. **Consider Scaling the Infrastructure**  
   - If the traffic increase is legitimate, **upgrade the VM** or **use AWS Auto Scaling**.

---

## **Final Verification**

- Monitor the VM again:
  ```bash
  free -h
  top
  ```
- Ensure **memory usage** is stable.
- Verify **NGINX is running smoothly**:
  ```bash
  sudo systemctl status nginx
  ```

By following these steps, you can **identify and fix** high memory usage in your NGINX load balancer VM. 🚀
