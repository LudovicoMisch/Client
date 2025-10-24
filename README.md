avviare una chat client-server in linguaggio html partendo dal login inserendo:
username
password
login
e il messaggio di accesso riusciuto o negato

spiegazione delle funzioni svolte:

// CREAZIONE DELLA CONNESSIONE WEBSOCKET CON IL SERVER IN ASCOLTO //
let ws = new WebSocket("ws://10.1.0.52:8090/chat25/5i1"); 

// RICEZIONE DEI MESSAGGI DAL SERVER E GESTIRE TRAMITE FUNZIONE SPECIFICA //
ws.onmessage = gestoreRicezione; 

// PRENDE I VALORI INSERITI DALL'UTENTE (nome e password) //
function loginServer() {
  let user = document.getElementById("nome").value;
  let pass = document.getElementById("password").value; 

  // COSTRUISCE UNA STRINGA DI LOGIN FORMATTATA PER IL SERVER ("log|utente|password") //
  let LOGIN = "log|" + user + "|" + pass; 

  // INVIA LA RICHIESTA DI LOGIN AL SERVER VIA WEBSOCKET //
  ws.send(LOGIN);
} 

 // RECUPERA L'AREA DI TESTO PER  MOSTRARE I MESSAGGI //
function gestoreRicezione(messaggioRicevuto) {
  let ricezione = document.getElementById("ricezione"); 

  // AGGIUNNGE IL MESSAGGIO RICEVUTO ALLA TEXTAREA PER LA VISUALIZZAZIONE CRONOLOGICA //
  ricezione.textContent += messaggioRicevuto.data + "\n"; 

  // SALVA IL CONTENUTO DEL MESSAGGIO RICEVUTO PER EVENTUALI CONTROLLI //
  let data = messaggioRicevuto.data;
  console.log(data); 

  // SE IL MESSAGGIO E' "LOGIN ERRATO", MOSTRA UN ALERT //
  if (data === "rlo|login errato") {
    alert("Login errato! Riprova.");
  } 

  // ALTRIMENTI, MOSTRA L'AREA CHAT E NASCONDE L'AREA LOGIN //
  else {
    document.getElementById("ricezione").textContent += data + "\n";
    document.getElementById("loginArea").style.display = "none";
    document.getElementById("chatArea").style.display = "block";
  }
} 

  // LEGGE IL MESSAGGIO SCRITTO DALL'UTENTE //
function inviaAlServer() {
  let messaggioDaInviare = document.getElementById("msg").value; 

  // PRENDE IL NOME UTENTE PERR INCLUDERLO NEL MESSAGGIO //
  let user = document.getElementById("nome").value; 

  // GENERA UN TIMESTAMP ISO PER MARCARE IL MOMENTO DEL MESSAGGIO //
  let timestamp = new Date().toISOString(); 

  // COSTRUISCE UN MESSAGGIO FORMATTATO "msg|utente|timestamp|messaggio" //
  let mes = "msg|" + user + "|" + timestamp + "|" + messaggioDaInviare; 

  // INVIA IL MESSAGGIO AL SERVER TRAMITE WEBSOCKET //
  ws.send(mes); 

  // SVUOTA IL CAMPO DI INPUT DEL MESSAGGIO DOPO L'INVIO //
  document.getElementById("msg").value = "";
} 

  // CHIUDE LA CONNESSIONE WEBSOCKET CON IL SERVER //
function chiudiLaConnessione() {
  ws.close(); 

  // NASCONDE L'AREA CHAT E MOSTRA QUELLA DI LOGIN PER PERMETTERE NUOVO ACCESSO //
  document.getElementById("chatArea").style.display = "none";
  document.getElementById("loginArea").style.display = "block";
} 
