<!DOCTYPE html>
<html>
<body>

<h2>AI Chat</h2>

<input id="inputUser" placeholder="Tanya..." style="width:75%">
<button onclick="kirim()">Kirim</button>

<pre id="hasil"></pre>

<script>
async function kirim() {

  const API_KEY = "AIzaSyAGO2kmtCKf-P79bvtCEze8eh8KJ2obC10";

  const prompt = `
Kamu adalah admin toko online.
Jawab singkat, ramah, dan jelas.
Gunakan bahasa Indonesia santai.
`;

  const userText = document.getElementById("inputUser").value;
  const finalText = prompt + "\nPembeli: " + userText;

  document.getElementById("hasil").innerText = "Mengetik...";

  const response = await fetch(
    "https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=" + API_KEY,
    {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        contents: [
          { parts: [{ text: finalText }] }
        ]
      })
    }
  );

  const data = await response.json();

  // 🔎 TAMPILKAN ERROR ASLI JIKA ADA
  if (!response.ok) {
    document.getElementById("hasil").innerText =
      "API ERROR:\n" + JSON.stringify(data, null, 2);
    return;
  }

  document.getElementById("hasil").innerText =
    data.candidates[0].content.parts[0].text;
}
</script>

</body>
</html>
