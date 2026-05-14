# Python Forensic Logging Tool with AES Encryption

## Overview
This Python-based keylogger captures keystrokes, logs them with timestamps, and encrypts the log file using **AES encryption** for security. The encryption ensures that recorded keystrokes remain confidential and can only be decrypted with the correct key.

## Features
✔ **Timestamp Logging** – Each keystroke is recorded with an accurate timestamp.  
✔ **AES Encryption** – Logs are encrypted for security using the `cryptography` library.  
✔ **Graceful Exit** – Press `ESC` to stop the keylogger safely.  
✔ **Secure Key Storage** – The encryption key is stored in a separate file.  
✔ **Immediate Data Flushing** – Ensures logs are written securely and immediately.  

## File Structure
```
/your_project_directory
  ├── keylogger.py          # Main script (Run this to start keylogging)
  ├── encryption_key.key    # Encryption key (Generated automatically)
  ├── keyfile_encrypted.txt # Encrypted log file
  ├── decrypt_log.py        # Decrypt logs (Run this to view keystrokes)
```

## Installation
### **1. Clone the Repository**
```bash
git clone https://github.com/BrandonRoos/KeyloggerEncryption.git
cd KeyloggerEncryption
```
### **2. Install Dependencies**
Make sure you have Python installed, then install the required libraries:
```bash
pip install pynput cryptography
```


## Usage
### **Start the Keylogger**
Run the `keylogger.py` script to start logging keystrokes:
```bash
python keylogger.py
```
> Press `ESC` to stop the keylogger.

### **Decrypt and View Logs**
To read the encrypted log file, run the `decrypt_log.py` script:
```bash
python decrypt_log.py
```
This will decrypt and display the recorded keystrokes.

## Code Explanation
### **1. `keylogger.py` (Main Keylogging Script)**
- Imports necessary modules: `pynput` for capturing keystrokes, `time` for timestamping, and `cryptography.fernet` for encryption.
- **Encryption Key Handling:**
  - If the key does not exist, it generates and saves a new AES encryption key.
  - This key is stored in `encryption_key.key` and used for encrypting logs.
- **Keystroke Logging:**
  - Listens for keypress events and logs them with a timestamp.
  - If a key is a regular character, it is logged directly.
  - Special keys like `ENTER`, `SPACE`, `BACKSPACE`, etc., are logged with descriptive names.
  - Each log entry is immediately encrypted and written to `keyfile_encrypted.txt`.
- **Listener:**
  - The program starts a listener that runs in the background until the `ESC` key is pressed, at which point it exits.

### **2. `decrypt_log.py` (Decryption Script)**
- Loads the AES encryption key from `encryption_key.key`.
- Reads the encrypted log file (`keyfile_encrypted.txt`).
- Decrypts and prints out the keystrokes in a readable format.
- Ensures that only authorized users with the correct key can access the logs.

## Security Notes
- **DO NOT share the `encryption_key.key` file**, as it is required to decrypt the logs.
- Without the key, the encrypted log file cannot be read.
- Use this script responsibly and ensure it is compliant with all legal and ethical guidelines.

## Future Enhancements
🔹 **Remote Logging** – Securely send keystrokes to a remote server.  
🔹 **GUI Interface** – Implement a graphical user interface for easier management.  
🔹 **Key Combination Detection** – Detect Ctrl+C, Ctrl+V, etc.  

## Disclaimer
This tool is intended for ethical and educational purposes only. Unauthorized use for malicious purposes is strictly prohibited. 

## License
[MIT License](LICENSE)

