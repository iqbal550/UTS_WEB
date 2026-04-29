# UTS_WEB

**Nama : M. Iqbal**
**NIm : 312410212**
**Kelas : I241B**


## Script.Js
// Memanggil library WebSocket
const WebSocket = require('ws');

// Membuat server di port 8080
const wss = new WebSocket.Server({ port: 8080 });

console.log("==========================================");
console.log("Server WebSocket Modern Berjalan...");
console.log("Alamat: ws://localhost:8080");
console.log("Status: Menunggu koneksi dari klien...");
console.log("==========================================");

// Event ketika ada klien yang terhubung
wss.on('connection', (ws) => {
    console.log("[INFO] Klien baru telah bergabung ke dalam chat.");

    // Mengirim pesan sambutan saat klien pertama kali terhubung
    ws.send("Sistem: Anda telah terhubung ke server WebSocket.");

    // Event ketika server menerima pesan dari klien
    ws.on('message', (data) => {
        // Mengonversi buffer data menjadi string agar bisa dibaca
        const messageStr = data.toString();
        console.log(`[PESAN TERIMA]: ${messageStr}`);

        // Mekanisme Broadcast: Mengirim pesan ke SEMUA klien yang terhubung
        wss.clients.forEach((client) => {
            if (client.readyState === WebSocket.OPEN) {
                client.send(messageStr);
            }
        });
    });

    // Event ketika klien memutus koneksi
    ws.on('close', () => {
        console.log("[INFO] Seorang klien telah keluar dari chat.");
    });

    // Penanganan error sederhana
    ws.on('error', (error) => {
        console.error(`[ERROR]: Terjadi kesalahan pada koneksi: ${error}`);
    });
});

## Index.Html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WebSocket Real-Time Chat</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body { background-color: #f4f7f6; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        #chatWindow { height: 400px; overflow-y: auto; background: white; border-radius: 10px; }
        .message { margin-bottom: 15px; padding: 10px; border-radius: 8px; max-width: 80%; }
        .received { background-color: #e9ecef; align-self: flex-start; }
        .sent { background-color: #007bff; color: white; align-self: flex-end; margin-left: auto; }
        .status-online { color: #28a745; font-size: 0.9rem; }
    </style>
</head>
<body>

<div class="container py-5">
    <div class="row justify-content-center">
        <div class="col-md-6">
            <div class="card shadow-sm">
                <div class="card-header bg-dark text-white d-flex justify-content-between align-items-center">
                    <h5 class="mb-0">WS Chat Experiment</h5>
                    <span id="status" class="badge bg-danger">Offline</span>
                </div>
                
                <div id="chatWindow" class="card-body d-flex flex-column p-3 border-bottom">
                    <p class="text-muted text-center small">Selamat datang di eksperimen WebSocket</p>
                </div>

                <div class="card-footer bg-white p-3">
                    <div class="input-group">
                        <input type="text" id="messageBox" class="form-control" placeholder="Tulis pesan..." onkeypress="handleKeyPress(event)">
                        <button class="btn btn-primary" onclick="sendMessage()">Kirim</button>
                    </div>
                </div>
            </div>
            <p class="text-center mt-3 text-secondary small">Eksperimen UTS Pemrograman Web</p>
        </div>
    </div>
</div>

<script>
    const socket = new WebSocket('ws://localhost:8080');
    const chatWindow = document.getElementById('chatWindow');
    const statusBadge = document.getElementById('status');

    socket.onopen = () => {
        statusBadge.textContent = "Online";
        statusBadge.className = "badge bg-success";
    };

    socket.onmessage = (event) => {
        displayMessage(event.data, 'received');
    };

    socket.onclose = () => {
        statusBadge.textContent = "Offline";
        statusBadge.className = "badge bg-danger";
    };

    function sendMessage() {
        const input = document.getElementById('messageBox');
        if (input.value.trim() !== "") {
            socket.send(input.value);
            // Opsional: Jika server tidak melakukan echo ke pengirim, tampilkan secara manual
            // displayMessage(input.value, 'sent'); 
            input.value = '';
        }
    }

    function displayMessage(text, type) {
        const msgDiv = document.createElement('div');
        msgDiv.className = `message ${type} shadow-sm px-3 py-2`;
        msgDiv.textContent = text;
        chatWindow.appendChild(msgDiv);
        
        // Auto scroll ke bawah
        chatWindow.scrollTop = chatWindow.scrollHeight;
    }

    function handleKeyPress(e) {
        if (e.key === 'Enter') sendMessage();
    }
</script>

</body>
</html>

## DEMO PROGRAM
<img width="1896" height="1094" alt="Screenshot 2026-04-29 130948" src="https://github.com/user-attachments/assets/cf10644e-ffc1-48fc-b6db-cb806ccb2616" />






