# 🔐 WEBLOCK - Password Manager Chrome Extension

Welcome to WEBLOCK, your new best friend in managing passwords! This Chrome extension generates, encrypts, and manages passwords with the power of AES encryption. And yes, it works on any Chromium-based browser. Say goodbye to 'password123' and hello to secure, encrypted passwords!

## 🌟 Features

- **Password Generation:** Because typing 'password123' isn't secure.
- **AES Encryption:** Like a vault for your secrets, but cooler and digital.
- **Secure Storage:** Stores encrypted passwords locally, so even your nosy cat can’t see them.
- **User-Friendly Interface:** So simple, even your grandma could use it. Well, almost.

## 🛠️ Built With

- **Frontend:** HTML, CSS, JavaScript
- **Backend:** Python, Flask, PyAES, Flask-CORS

## 📦 Installation

### Prerequisites

- Python 3.x
- A Chromium-based browser (e.g., Chrome, Edge, Brave)

### Steps

1. **Clone the Repository:**
    ```sh
    git clone https://github.com/MoriMidoriya/WEBLOCK.git
    cd WEBLOCK
    ```

2. **Install Backend Dependencies:**
    ```sh
    pip install flask pyaes flask-cors
    ```

3. **Load the Extension in Your Browser:**
    1. Open your Chromium-based browser and go to `chrome://extensions/`.
    2. Enable "Developer mode" by clicking the toggle switch in the top right corner.
    3. Click "Load unpacked" and select the directory where you cloned the repository.

## 🚀 Usage

### Server Side

1. **Run the Server:**
    ```sh
    python server.py
    ```
    This starts the server on `http://localhost:5000`, which handles encryption and decryption requests. (You can change the port in `server.py` if needed.)

### Client Side

1. **Open the Extension:**
    - Click on the WEBLOCK icon in the browser toolbar. It’s the shiny one.

2. **Generate and Save Password:**
    - Enter the username and customize the password options.
    - Click "Generate Password" to create a new password.
    - Click "Save" to store the encrypted password along with the username and the website URL.

3. **View Saved Logins:**
    - Click "Show Saved Logins" to view the saved logins.
    - Enter your encryption key to decrypt and view the passwords. And voila! Your secrets are revealed (but only to you).

## 🔄 How It Works (aka The Magical Journey of Your Password)

### Process Involved

Here's a step-by-step rundown of the wizardry:

1. **Password Generation:**
   - You input a username and customize your password options (alphabets, numbers, special characters) in the extension popup.
   - Click "Generate Password" and `generatePassword()` in `popup.js` conjures up a random password.

2. **Encryption Request: (Skip to Step 7 if you're not a nerd. Do that even if you ARE a nerd, idk why I added this)**
   - Click "Save", and the magic begins. Your generated password and username are sent to the server for encryption.
   - `encryptPassword()` sends a POST request to our friendly Flask server’s `/encrypt` endpoint with the plaintext password.
   - Example POST request:
     ```javascript
     const response = await fetch('http://localhost:5000/encrypt', {
         method: 'POST',
         headers: {
             'Content-Type': 'application/json'
         },
         body: JSON.stringify({ plaintext })
     });
     ```

3. **Server-Side Encryption:**
   - The Flask server receives the request and processes it in the `/encrypt` endpoint.
   - Your plaintext password is encrypted using AES, and both the ciphertext and encryption key are sent back to you.
   - Example Python code:
     ```python
     @app.route('/encrypt', methods=['POST'])
     def encrypt():
         data = request.json
         plaintext = data['plaintext']
         key = generate_key()
         aes = pyaes.AES(key)
         ciphertext = aes.encrypt(plaintext.encode())
         return jsonify({'ciphertext': ciphertext.hex(), 'key': key.decode()})
     ```

4. **Storage:**
   - The encrypted password, username, and encryption key are stored in your browser's local storage.
   - The key is shown to you in a modal—don’t lose it! It’s the VIP pass to your secrets.

5. **Decryption Request:**
   - To view a saved password, provide your encryption key.
   - `decryptPassword()` sends a POST request to the Flask server’s `/decrypt` endpoint with the ciphertext and your key.
   - Example POST request:
     ```javascript
     const response = await fetch('http://localhost:5000/decrypt', {
         method: 'POST',
         headers: {
             'Content-Type': 'application/json'
         },
         body: JSON.stringify({ ciphertext, key: providedKey, storedKey })
     });
     ```

6. **Server-Side Decryption:**
   - The Flask server checks your key and, if it matches, decrypts the password and sends it back to you.
   - Example Python code:
     ```python
     @app.route('/decrypt', methods=['POST'])
     def decrypt():
         data = request.json
         provided_key = data['key'].encode()
         stored_key = data['storedKey'].encode()
         ciphertext = bytes.fromhex(data['ciphertext'])
         if provided_key != stored_key:
             return jsonify({'error': 'Wrong key'}), 401
         aes = pyaes.AES(provided_key)
         decrypted = aes.decrypt(ciphertext)
         return jsonify({'decrypted': decrypted.decode()})
     ```

7. **Display Decrypted Password:**
   - The decrypted password is displayed in the extension popup, allowing you to see your login details. And voilà, you’re in!

## 📋 File Descriptions

### `manifest.json`

Defines the extension's configuration, including the name, version, and permissions required.

### `popup.html`

The HTML structure for the extension's popup interface. It’s where the magic begins.

### `popup.css`

Styles for the extension's popup interface. Keeping it stylish and user-friendly.

### `popup.js`

JavaScript code handling the extension's functionality, including password generation, encryption, and server communication.

### `server.py`

Python server-side code using Flask to handle encryption and decryption requests via AES.

- **Key Functions:**
  - `generate_key()`: Generates a random 32-byte key.
  - `/encrypt`: Endpoint to encrypt the plaintext password.
  - `/decrypt`: Endpoint to decrypt the ciphertext using the provided key.

## 🖼️ Screenshots

![Home](https://github.com/MoriMidoriya/WEBLOCK/assets/91540376/d4c35639-8b83-4788-9007-5702652874f8)
![Save](https://github.com/MoriMidoriya/WEBLOCK/assets/91540376/7e38759d-572c-4c2d-a9f0-ebff4b04ab75)


## 📞 Contact

For any queries or issues, feel free to reach out or open an issue in the repository. I don’t bite, that's a promise.

---

Thank you for using the WEBLOCK Password Manager Chrome Extension! 🎉
