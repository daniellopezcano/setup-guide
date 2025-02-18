# via fma.if.usp.br

## **1️ Generate SSH Keys on Your Local Machine**
Create your SSH keys, run:
```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_fma
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_kosmivara3110
```
- These will generate **two key pairs** (`.pub` is the public key):
    - `id_ed25519_fma` → For logging into `fma`
    - `id_ed25519_kosmivara3110` → For logging into `kosmivara3110` **via** `fma`

## **2️ Manually Copy Public Key to fma**
1. Display the contents of your **public key** (`id_ed25519_fma.pub`):
    ```bash
    cat ~/.ssh/id_ed25519_fma.pub
    ```
2. Copy the output of the command to your clipboard.
3. Connect to `fma` using your password:
    ```bash
    ssh dlopez@fma.if.usp.br
    ```
4. Once inside `fma`, **append** the public key to `~/.ssh/authorized_keys`:
    ```bash
    nano ~/.ssh/authorized_keys
    ```
    - Paste the copied key at the end of the file.
    - Save and exit (`CTRL+X`, then `Y`, then `Enter`).
5. Set proper permissions:
    ```bash
    chmod 600 ~/.ssh/authorized_keys
    chmod 700 ~/.ssh
    ```
6. Now, **exit** fma
    ```bash
    exit
    ssh dlopez@fma.if.usp.br
    ```
7. In your local file add the following lines to your config file (~/.ssh/config)
    ```bash
	Host fma
	    HostName fma.if.usp.br
	    User dlopez
	    ForwardAgent yes
	    ServerAliveInterval 60
	    ServerAliveCountMax 10
	    IdentityFile ~/.ssh/id_ed25519_fma
    ```
8. Try to access *jump* (fma) from *local* automatically without the password doing:
    ```bash
    ssh fma
    ```

## **3️ Manually Copy Public Key to kosmivara3110**

1. Display the **public key** for `kosmivara3110`:
    ```bash
    cat ~/.ssh/id_ed25519_kosmivara3110.pub
    ```
2. Copy the output to your clipboard.
3. **Log into `fma`**, then manually SSH into `kosmivara3110`:
    ```bash
    ssh fma
    ssh dlopez@10.1.0.101
    ```
4. Inside `kosmivara3110`, edit the `authorized_keys` file:
    ```bash
    nano ~/.ssh/authorized_keys
    ```
    - **Paste** the copied public key at the end of the file.
    - Save and exit (`CTRL+X`, then `Y`, then `Enter`).
5. Set proper permissions:    
    ```bash
    chmod 600 ~/.ssh/authorized_keys
    chmod 700 ~/.ssh
    ```
6. Exit both machines

## **4️ Configure Local SSH for ProxyJump**

Now, configure your **local** SSH `config` file to automate the connection to *target* (kosmivara3110).
Edit `~/.ssh/config` on your local machine:
```bash
nano ~/.ssh/config
```
Add the following lines to the local config file for connecting directly  to *target* (kosmivara3110) through *jump* (fma):
```
Host kosmivara3110
    HostName 10.1.0.101
    User dlopez
    IdentityFile ~/.ssh/id_ed25519_kosmivara3110
    ProxyCommand ssh -W %h:%p fma
    ForwardAgent yes
```
Save and exit

## **5️ Test the Connection**
Now, from your **local** machine, try:
```bash
ssh kosmivara3110
```
- This should is basically asking *jump* (fma) to fetch the local private key for *target* (kosmivara3110) to access without requiring a password



# via DynahostNIS

## **Prerequisites**

- An active IFUSP email account with login credentials.
- An SSH client installed (Linux/macOS: OpenSSH; Windows: PuTTY, OpenSSH, or MobaXterm).
- A valid username and password for DynahostNIS (ask someone from CCIFUSP).

---

## **Registering Your IP with IFUSP via DynahostNIS**

To access internal IFUSP network machines, you must register your external IP using DynahostNIS. Follow these steps:

### **Step 1: Open the DynahostNIS Registration Page**

7. Open a browser and navigate to:  
    **[DynahostNIS Registration](http://fmatrm.if.usp.br/dynahostnis.html)**
8. You will see a page prompting you to discover your current IP.
9. Click on the button:  
    **"Clique aqui para descobrir o endereço IP corrente do seu sistema remoto"**

### **Step 2: Obtain Your External IP**

After clicking the button, you will be redirected to a new page displaying your **current IP address**. The page should look similar to this:

```
Cadastro de Sistemas Remotos com IP Dinâmico

Este formulário deve ser preenchido a partir de um browser rodando em seu sistema residencial ou remoto. Veja abaixo o endereço IP corrente do seu sistema remoto.

143.107.129.13

Capture o número com o seu mouse e guarde-o para poder usá-lo no formulário encriptado.
```

- Copy and **save this IP address**; you will need it for the next step.
- Click on **"Formulário encriptado"** to proceed to the final registration form.

### **Step 3: Register Your IP Address**

10. In the final form, you will see two fields:
    - **Hostname**: Leave this field **empty**.
    - **Endereço** (Address): Paste the IP address you copied in Step 2.
11. Click **"SUBMETER"** to submit your IP for registration.
12. You should now be registered in the IFUSP network.

### **Credentials for Registration:**
use the ones the people from CCIFUSP have provided

---

## **Verifying Connection to IFUSP Network**

Once your IP is registered, your computer is now effectively **inside the IFUSP network**. You should be able to connect directly to other IFUSP machines.

### **Testing SSH Connection to ****`kosmivara3110`**

From your terminal, run:

```bash
ssh dlopez@10.1.0.101
```

- If successful, you should **not** need to use a jump host (`fma.if.usp.br`) anymore.

### **Configuring Direct Access via VS Code**

13. **Open VS Code**.
14. **Go to the Extensions Marketplace (****`Ctrl+Shift+X`****)**.
15. **Search and Install** `Remote - SSH`.
16. **Open the Command Palette (****`Ctrl+Shift+P`****)**.
17. Select **"Remote-SSH: Add New SSH Host"**.
18. Enter the connection string:
    
    ```bash
    ssh dlopez@10.1.0.101
    ```
    
19. **Press Enter** and save it to your SSH configuration.
20. **Go back to Command Palette** and select **"Remote-SSH: Connect to Host"**.
21. Choose `kosmivara3110` from the list.
22. **VS Code will open a new window, directly connected to ****`kosmivara3110`****.**
