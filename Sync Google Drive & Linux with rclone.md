## **1. Installing rclone**
`rclone` is a command-line tool that allows interaction with cloud storage services. In this case, it is used to connect Google Drive with a Debian system.
### **Command used**
```bash
sudo apt update
sudo apt install rclone
```
---
## **2. Configuring rclone**
A "remote" was configured in `rclone` to connect to Google Drive.
### **Steps followed**
1. Run the configuration command:
    ```bash
    rclone config
    ```
2. In the configuration wizard:
    - Select `n` to create a new configuration.
    - Assign the name `gdrive` to the remote.
    - Choose `drive` as the storage type.
    - Authenticate `rclone` with your Google account using a provided link.
3. Verify the connection by listing files in Google Drive:
    ```bash
    rclone ls gdrive:
    ```
---
## **3. Manual Folder Synchronization**
A manual synchronization of a local folder with a remote folder in Google Drive was performed.
### **Command used**
```bash
rclone sync /home/dlopez/Documents/1.personal gdrive:1.personal --log-file=/home/dlopez/logs/rclone_1.personal.log --log-level INFO
```
### **Command Explanation**
- `/home/dlopez/Documents/1.personal`: Local folder to synchronize.
- `gdrive:1.personal`: Remote folder in Google Drive where data is synchronized.
- `--log-file=/home/dlopez/logs/rclone_1.personal.log`: Specifies a file where the process logs are saved.
- `--log-level INFO`: Sets the log detail level.

---

## **4. Automating Synchronization with cron**
To perform automatic synchronizations, periodic tasks were configured using `cron`.
### **Steps followed**
1. Edit the scheduled tasks:
    ```bash
    crontab -e
    ```
2. Add the following lines to the `crontab` file:
    ```bash
*/5 * * * * rclone sync /home/dlopez/Documentos/1.personal gdrive:1.personal --log-file=/home/dlopez/logs/rclone_1.personal.log --log-level INFO
*/5 * * * * rclone sync /home/dlopez/Documentos/2.documentacion gdrive:2.documentacion --log-file=/home/dlopez/logs/rclone_2.documentacion.log --log-level INFO
*/5 * * * * rclone sync /home/dlopez/Documentos/obsidian gdrive:obsidian --log-file=/home/dlopez/logs/rclone_obsidian.log --log-level INFO
0 3 * * * rclone sync /home/dlopez/Documentos/1.personal gdrive:1.personal --log-file=/home/dlopez/logs/rclone_1.personal.log --log-level INFO || echo "Error: Google Drive sync 1.personal" | mail -s "rclone ERROR 1.personal" daniellopezcano13@gmail.com
0 3 * * * rclone sync /home/dlopez/Documentos/2.documentacion gdrive:2.documentacion --log-file=/home/dlopez/logs/rclone_2.documentacion.log --log-level INFO || echo "Error: Google Drive sync  2.documentacion" | mail -s "rclone ERROR 2.documentacion" daniellopezcano13@gmail.com
0 3 * * * rclone sync /home/dlopez/Documentos/obsidian gdrive:obsidian --log-file=/home/dlopez/logs/rclone_obsidian.log --log-level INFO || echo "Error: Google Drive sync  obsidian" | mail -s "rclone ERROR obsidian" daniellopezcano13@gmail.com

    ```
    ### **Explanation of the lines**
- `*/5 * * * *`: Executes the command each 5 minutes.
- `rclone sync`: Command to synchronize folders.
- `--log-file`: Saves logs to a specific file.
- `--log-level INFO`: Ensures relevant synchronization details are logged.
---
## **5. Verifying the Functionality**
To ensure synchronization works correctly:
1. Manually execute one of the synchronization commands:
    ```bash
    rclone sync /home/dlopez/Documents/1.personal gdrive:1.personal --log-file=/home/dlopez/logs/rclone_1.personal.log --log-level INFO
    ```
2. Check the contents of the log file:
    ```
    tail -f ~/logs/rclone_1.personal.log
    ```
### **Example Log Output**
```
ransferred:   	          0 B / 0 B, -, 0 B/s, ETA -
Checks:               442 / 442, 100%
Elapsed time:         2.9s

2025/01/17 10:15:04 INFO  : There was nothing to transfer
2025/01/17 10:15:04 INFO  : 
Transferred:   	          0 B / 0 B, -, 0 B/s, ETA -
Checks:               442 / 442, 100%
Elapsed time:         2.8s

2025/01/17 10:20:05 INFO  : guitarra/~$tensiones.pptx: Deleted
2025/01/17 10:20:05 INFO  : guitarra/~$guitar_music_theory_poster.pptx: Deleted
2025/01/17 10:20:05 INFO  : There was nothing to transfer
2025/01/17 10:20:05 INFO  : 
Transferred:   	          0 B / 0 B, -, 0 B/s, ETA -
Checks:               442 / 442, 100%
Deleted:                2 (files), 0 (dirs)
Elapsed time:         3.4s
```






