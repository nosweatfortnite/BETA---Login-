<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Battlecraft Studios – Service Line</title>
<style>
  body { background:#0e0e0e; color:#fff; font-family:Arial, sans-serif; margin:0; padding:20px; }
  .wrap { max-width: 980px; margin: 0 auto; }
  h1 { text-align:center; color:#ffcc00; margin: 10px 0 20px; }
  .card { background:#1a1a1a; border:1px solid #444; border-radius:12px; padding:18px; margin: 12px 0; }
  label { display:block; margin:10px 0 6px; color:#ddd; }
  input, textarea, select {
    width:100%; box-sizing:border-box;
    background:#111; border:1px solid #444; color:#fff;
    border-radius:10px; padding:10px; outline:none;
  }
  textarea { min-height:120px; resize:vertical; }
  .row { display:flex; gap:12px; flex-wrap:wrap; }
  .row > * { flex:1; min-width: 220px; }
  button {
    background:#ffcc00; color:#000; border:none;
    padding:10px 14px; border-radius:10px; cursor:pointer; font-weight:bold;
  }
  button:hover { filter: brightness(0.95); }
  .muted { color:#aaa; font-size:13px; line-height:1.4; }
  .pill { display:inline-block; padding:6px 10px; border-radius:999px; border:1px solid #444; background:#111; color:#ddd; font-size:13px; }
  .danger { background:#2b1f1f; border-color:#7a2a2a; }
  .dangerText { color:#ff6b6b; font-weight:bold; }
  .successText { color:#6bff9c; font-weight:bold; }
  .hidden { display:none; }
  hr { border:0; border-top:1px solid #333; margin:14px 0; }
  .docItem { padding:10px; border:1px solid #333; border-radius:10px; margin:10px 0; background:#121212; }
  .docTitle { font-weight:bold; color:#ffcc00; }
  .topbar { display:flex; align-items:center; justify-content:space-between; gap:12px; flex-wrap:wrap; }
</style>
</head>
<body>
<div class="wrap">

  <h1>Battlecraft Studios – Service Line</h1>

  <!-- BAN SCREEN -->
  <div id="banScreen" class="card danger hidden">
    <h2 class="dangerText">🚫 You have been banned</h2>
    <p id="banMsg" style="line-height:1.6;"></p>
    <p class="muted">
      If you believe this is a mistake, contact a moderator.
    </p>
    <button onclick="logout()">Return to Login</button>
  </div>

  <!-- AUTH -->
  <div id="authScreen" class="card">
    <div class="topbar">
      <div>
        <div class="pill">Local Prototype</div>
        <div class="muted">Accounts/files are saved in <b>this browser only</b> (localStorage).</div>
      </div>
    </div>

    <hr>

    <div class="row">
      <div class="card" style="margin:0;">
        <h2 style="margin:0 0 8px; color:#ffcc00;">Login</h2>
        <label>Username</label>
        <input id="loginUser" placeholder="e.g. Jerry Winterson" />
        <label>Password</label>
        <input id="loginPass" type="password" placeholder="e.g. 919" />
        <div style="margin-top:12px;" class="row">
          <button onclick="login()">Login</button>
          <button onclick="fillMod()">Fill Mod 1</button>
        </div>
        <p id="authError" class="dangerText" style="margin-top:10px;"></p>
      </div>

      <div class="card" style="margin:0;">
        <h2 style="margin:0 0 8px; color:#ffcc00;">Create Account</h2>
        <label>Username</label>
        <input id="regUser" placeholder="Choose a username" />
        <label>Password</label>
        <input id="regPass" type="password" placeholder="Choose a password" />
        <label>Starting Funds</label>
        <input id="regFunds" type="number" value="0" min="0" />
        <div style="margin-top:12px;">
          <button onclick="register()">Create Account</button>
        </div>
        <p class="muted" style="margin-top:10px;">
          Tip: this is a demo. For real website-wide accounts/bans you’ll need a database/server.
        </p>
      </div>
    </div>
  </div>

  <!-- USER DASHBOARD -->
  <div id="userScreen" class="hidden">
    <div class="card">
      <div class="topbar">
        <div>
          <div style="font-size:18px;"><b id="who"></b></div>
          <div class="muted">
            Funds: <span class="pill" id="funds"></span>
            <span class="pill" id="rolePill"></span>
          </div>
        </div>
        <div class="row" style="flex:0; min-width: 280px;">
          <button onclick="logout()">Logout</button>
        </div>
      </div>
    </div>

    <!-- DOCS -->
    <div class="card">
      <h2 style="margin:0 0 10px; color:#ffcc00;">📁 Your Documents</h2>
      <div class="row">
        <div>
          <label>Document Title</label>
          <input id="docTitle" placeholder="e.g. My Notes" />
        </div>
        <div>
          <label>Visibility</label>
          <select id="docVisibility">
            <option value="private">Private (only you)</option>
            <option value="mods">Mods can view</option>
          </select>
        </div>
      </div>
      <label>Document Content</label>
      <textarea id="docContent" placeholder="Write anything here..."></textarea>
      <div style="margin-top:12px;" class="row">
        <button onclick="saveDoc()">Save Document</button>
        <button onclick="clearDocFields()">Clear</button>
      </div>

      <hr>
      <div id="docsList"></div>
    </div>

    <!-- MOD PANEL -->
    <div id="modPanel" class="card hidden">
      <h2 style="margin:0 0 10px; color:#ffcc00;">🛡️ Moderator Panel</h2>
      <p class="muted" style="margin-top:0;">
        Moderator Account 1 is built-in: <b>Jellyfish24</b> / <b>TZ6789</b>
      </p>

      <div class="row">
        <div>
          <label>Find User by Username</label>
          <input id="modFindUser" placeholder="Type the exact username" />
        </div>
        <div>
          <label>Action</label>
          <select id="modAction" onchange="renderModAction()">
            <option value="view">View User</option>
            <option value="funds">Set Funds</option>
            <option value="ban">Ban User</option>
            <option value="unban">Unban User</option>
          </select>
        </div>
      </div>

      <div id="modActionArea"></div>

      <div style="margin-top:12px;" class="row">
        <button onclick="modExecute()">Execute</button>
      </div>

      <p id="modMsg" style="margin-top:12px;"></p>
    </div>
  </div>

</div>

<script>
/**
 * STORAGE MODEL (local prototype):
 * db = { users: { username: { pass, funds, role, docs: [], banned: {until, reason} | null } } }
 * session = { user: "name" }
 */

const MOD_ACCOUNTS = [
  { user: "Jellyfish24", pass: "TZ6789", role: "mod" }
];

function loadDB() {
  const raw = localStorage.getItem("bc_db");
  if (!raw) return { users: {} };
  try { return JSON.parse(raw); } catch { return { users: {} }; }
}
function saveDB(db) {
  localStorage.setItem("bc_db", JSON.stringify(db));
}

function setSession(username) {
  localStorage.setItem("bc_session", JSON.stringify({ user: username }));
}
function getSession() {
  const raw = localStorage.getItem("bc_session");
  if (!raw) return null;
  try { return JSON.parse(raw); } catch { return null; }
}
function clearSession() {
  localStorage.removeItem("bc_session");
}

function nowMs() { return Date.now(); }
function daysToMs(days) { return Math.max(0, Number(days || 0)) * 24 * 60 * 60 * 1000; }

function ensureModsInDB(db) {
  for (const m of MOD_ACCOUNTS) {
    if (!db.users[m.user]) {
      db.users[m.user] = { pass: m.pass, funds: 0, role: "mod", docs: [], banned: null };
    } else {
      db.users[m.user].role = "mod";
      // Keep existing funds/docs, but ensure pass matches if you want strict:
      db.users[m.user].pass = m.pass;
    }
  }
  saveDB(db);
}

function show(elId, on=true) {
  const el = document.getElementById(elId);
  if (!el) return;
  el.classList.toggle("hidden", !on);
}

function fillMod() {
  document.getElementById("loginUser").value = "Jellyfish24";
  document.getElementById("loginPass").value = "TZ6789";
}

function register() {
  const user = document.getElementById("regUser").value.trim();
  const pass = document.getElementById("regPass").value;
  const funds = Number(document.getElementById("regFunds").value || 0);

  const errEl = document.getElementById("authError");
  errEl.textContent = "";

  if (!user || !pass) {
    errEl.textContent = "Please enter a username and password.";
    return;
  }

  const db = loadDB();
  ensureModsInDB(db);

  if (db.users[user]) {
    errEl.textContent = "That username already exists.";
    return;
  }

  db.users[user] = { pass, funds: Math.max(0, funds), role: "user", docs: [], banned: null };
  saveDB(db);

  document.getElementById("regUser").value = "";
  document.getElementById("regPass").value = "";
  document.getElementById("regFunds").value = "0";

  errEl.textContent = "";
  alert("Account created! You can log in now.");
}

function login() {
  const user = document.getElementById("loginUser").value.trim();
  const pass = document.getElementById("loginPass").value;

  const errEl = document.getElementById("authError");
  errEl.textContent = "";

  if (!user || !pass) {
    errEl.textContent = "Enter username and password.";
    return;
  }

  const db = loadDB();
  ensureModsInDB(db);

  const u = db.users[user];
  if (!u || u.pass !== pass) {
    errEl.textContent = "Invalid username or password.";
    return;
  }

  // check ban
  if (u.banned && u.banned.until && nowMs() < u.banned.until) {
    setSession(user);
    render();
    return;
  } else if (u.banned && u.banned.until && nowMs() >= u.banned.until) {
    // auto-unban after time
    u.banned = null;
    saveDB(db);
  }

  setSession(user);
  render();
}

function logout() {
  clearSession();
  render();
}

function render() {
  const db = loadDB();
  ensureModsInDB(db);

  const session = getSession();
  if (!session || !session.user || !db.users[session.user]) {
    show("authScreen", true);
    show("userScreen", false);
    show("banScreen", false);
    document.getElementById("authError").textContent = "";
    return;
  }

  const current = db.users[session.user];

  // If banned, show ban screen
  if (current.banned && current.banned.until && nowMs() < current.banned.until) {
    show("authScreen", false);
    show("userScreen", false);
    show("banScreen", true);

    const untilDate = new Date(current.banned.until);
    document.getElementById("banMsg").innerHTML =
      `<span class="dangerText">You have been banned from Battlecraft Studios's service line</span><br><br>` +
      `<b>Reason:</b> ${escapeHTML(current.banned.reason || "No reason provided")}<br>` +
      `<b>Unban date:</b> ${untilDate.toLocaleString()}`;
    return;
  }

  // Normal dashboard
  show("authScreen", false);
  show("banScreen", false);
  show("userScreen", true);

  document.getElementById("who").textContent = `Welcome, ${session.user}`;
  document.getElementById("funds").textContent = `${current.funds ?? 0}`;
  document.getElementById("rolePill").textContent = (current.role === "mod") ? "Moderator" : "User";

  // docs
  renderDocs(current);

  // mod panel
  const isMod = current.role === "mod";
  show("modPanel", isMod);
  if (isMod) {
    renderModAction();
  }
}

function escapeHTML(str) {
  return String(str)
    .replaceAll("&", "&amp;")
    .replaceAll("<", "&lt;")
    .replaceAll(">", "&gt;")
    .replaceAll('"', "&quot;")
    .replaceAll("'", "&#039;");
}

/* ---------- Docs ---------- */
function saveDoc() {
  const title = document.getElementById("docTitle").value.trim();
  const content = document.getElementById("docContent").value;
  const visibility = document.getElementById("docVisibility").value;

  if (!title) { alert("Please enter a document title."); return; }

  const db = loadDB();
  const session = getSession();
  if (!session || !db.users[session.user]) return;

  const user = db.users[session.user];
  user.docs = user.docs || [];

  // overwrite if same title exists
  const idx = user.docs.findIndex(d => d.title.toLowerCase() === title.toLowerCase());
  const docObj = { title, content, visibility, updatedAt: nowMs() };

  if (idx >= 0) user.docs[idx] = docObj;
  else user.docs.unshift(docObj);

  saveDB(db);
  clearDocFields();
  render();
}

function clearDocFields() {
  document.getElementById("docTitle").value = "";
  document.getElementById("docContent").value = "";
  document.getElementById("docVisibility").value = "private";
}

function deleteDoc(title) {
  const db = loadDB();
  const session = getSession();
  if (!session || !db.users[session.user]) return;

  const user = db.users[session.user];
  user.docs = (user.docs || []).filter(d => d.title !== title);

  saveDB(db);
  render();
}

function loadDoc(title) {
  const db = loadDB();
  const session = getSession();
  if (!session || !db.users[session.user]) return;

  const user = db.users[session.user];
  const doc = (user.docs || []).find(d => d.title === title);
  if (!doc) return;

  document.getElementById("docTitle").value = doc.title;
  document.getElementById("docContent").value = doc.content;
  document.getElementById("docVisibility").value = doc.visibility || "private";
  window.scrollTo({ top: 0, behavior: "smooth" });
}

function renderDocs(user) {
  const list = document.getElementById("docsList");
  const docs = user.docs || [];

  if (docs.length === 0) {
    list.innerHTML = `<p class="muted">No documents yet. Create one above.</p>`;
    return;
  }

  list.innerHTML = docs.map(d => {
    const when = new Date(d.updatedAt || nowMs()).toLocaleString();
    return `
      <div class="docItem">
        <div class="docTitle">${escapeHTML(d.title)}</div>
        <div class="muted">Visibility: ${escapeHTML(d.visibility || "private")} • Updated: ${when}</div>
        <div style="margin-top:10px;" class="row">
          <button onclick="loadDoc('${escapeHTML(d.title).replaceAll("&#039;","\\'")}')">Open</button>
          <button onclick="deleteDoc('${escapeHTML(d.title).replaceAll("&#039;","\\'")}')">Delete</button>
        </div>
      </div>
    `;
  }).join("");
}

/* ---------- Moderator ---------- */
function renderModAction() {
  const action = document.getElementById("modAction").value;
  const area = document.getElementById("modActionArea");

  if (action === "view") {
    area.innerHTML = `<p class="muted">Shows user info and (if allowed) documents visible to mods.</p>`;
  } else if (action === "funds") {
    area.innerHTML = `
      <label>New Funds Amount</label>
      <input id="modFunds" type="number" value="0" min="0" />
    `;
  } else if (action === "ban") {
    area.innerHTML = `
      <label>Ban Reason</label>
      <input id="modReason" placeholder="Type any reason..." />
      <label>Ban Length (days)</label>
      <input id="modDays" type="number" value="1" min="1" />
      <p class="muted">User will see a red ban screen until the ban expires.</p>
    `;
  } else if (action === "unban") {
    area.innerHTML = `<p class="muted">Removes the ban immediately.</p>`;
  }
}

function modExecute() {
  const db = loadDB();
  const session = getSession();
  if (!session || !db.users[session.user] || db.users[session.user].role !== "mod") return;

  const targetName = document.getElementById("modFindUser").value.trim();
  const msg = document.getElementById("modMsg");
  msg.textContent = "";
  msg.className = "";

  if (!targetName) {
    msg.textContent = "Type a username to target.";
    msg.className = "dangerText";
    return;
  }
  if (!db.users[targetName]) {
    msg.textContent = "That user does not exist (in this browser).";
    msg.className = "dangerText";
    return;
  }

  const action = document.getElementById("modAction").value;
  const target = db.users[targetName];

  if (action === "view") {
    const canSeeDocs = (target.docs || []).filter(d => d.visibility === "mods");
    const banInfo = target.banned && target.banned.until && nowMs() < target.banned.until
      ? `BANNED until ${new Date(target.banned.until).toLocaleString()} (Reason: ${target.banned.reason || "n/a"})`
      : "Not banned";

    msg.innerHTML =
      `<span class="successText">User:</span> ${escapeHTML(targetName)}<br>` +
      `<span class="successText">Role:</span> ${escapeHTML(target.role || "user")}<br>` +
      `<span class="successText">Funds:</span> ${escapeHTML(target.funds ?? 0)}<br>` +
      `<span class="successText">Status:</span> ${escapeHTML(banInfo)}<br>` +
      `<br><span class="successText">Docs visible to mods:</span> ${canSeeDocs.length}`;
    return;
  }

  if (action === "funds") {
    const newFunds = Math.max(0, Number(document.getElementById("modFunds").value || 0));
    target.funds = newFunds;
    saveDB(db);
    msg.textContent = `Funds updated for ${targetName} → ${newFunds}`;
    msg.className = "successText";
    render();
    return;
  }

  if (action === "ban") {
    const reason = document.getElementById("modReason").value.trim() || "No reason provided";
    const days = Math.max(1, Number(document.getElementById("modDays").value || 1));
    target.banned = { reason, until: nowMs() + daysToMs(days) };
    saveDB(db);
    msg.textContent = `Banned ${targetName} for ${days} day(s).`;
    msg.className = "successText";
    render(); // if banning self in testing, it updates
    return;
  }

  if (action === "unban") {
    target.banned = null;
    saveDB(db);
    msg.textContent = `Unbanned ${targetName}.`;
    msg.className = "successText";
    render();
    return;
  }
}

(function init() {
  const db = loadDB();
  ensureModsInDB(db);
  render();
})();
</script>

</body>
</html>
