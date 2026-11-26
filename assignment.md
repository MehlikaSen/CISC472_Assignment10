**Hosted application:** [https://cisc472-assignment10.onrender.com](https://cisc472-assignment10.onrender.com)

# OS Command Injection

**Location:** `server.js`, `app.post("/")` (lines 76–81)

**Trigger:** Submitting crafted input in the Host field. The value is interpolated directly into a shell command without validation or being passed as a separate argument.

*Example:* For a harmless local demonstration, enter `127.0.0.1;` in the Host field. The semicolon is passed into the shell command even though the form is intended to accept only a hostname or IP address; this trailing semicolon runs no additional command. The endpoint does not require an account.

**The Fix**: Validate that the submitted value is a hostname or IP address, then invoke `ping` without a shell (for example, with Node.js `execFile` and separate arguments). This prevents shell syntax in the input from being interpreted as commands while preserving normal host checks.



**What process sends the check:** The Node.js server running the Express app receives the form submission and starts the system `ping` process using `child_process.exec`. The ping command runs in the app's environment.

**What the form accepts:** The page asks for a host name or IP address and submits it as the `host` field. The server trims the submitted value and rejects an empty value, but it does not validate that the value is actually a hostname or IP address before using it.

**What a reply looks like when the host answers and when it does not:** If the ping command succeeds, the page displays the host, an “up” status, and the command output. If it fails or times out, the page displays a “down” status and the error or ping output. The result is displayed in the response page; it is not stored as a persistent check history. The “Recent checks” rows are examples hard-coded in the server.

**What else is in the repo that the page does not link:** `desk.txt` is a project file that the page does not link to. The README describes it as a desk note.

