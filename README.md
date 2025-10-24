avviare una chat client-server in linguaggio html partendo dal login inserendo:
username
password
login
e il messaggio di accesso riusciuto o negato

spiegazione delle funzioni svolte:

// Creazione della connessione WebSocket con il server in ascolto //
let ws = new WebSocket("ws://10.1.0.52:8090/chat25/5i1"); 

// Ricezione dei messaggi dal server e gestione tramite funzione specifica //
ws.onmessage = gestoreRicezione; 

// Prende i valori inseriti dall’utente (nome e password) //
function loginServer() {
  let user = document.getElementById("nome").value;
  let pass = document.getElementById("password").value; 

  // Costruisce una stringa di login formattata per il server ("log|utente|password") //
  let LOGIN = "log|" + user + "|" + pass; 

  // Invia la richiesta di login al server via WebSocket //
  ws.send(LOGIN);
} 

 // Recupera l'area di testo per mostrare i messaggi //
function gestoreRicezione(messaggioRicevuto) {
  let ricezione = document.getElementById("ricezione"); 

  // Aggiunge il messaggio ricevuto alla textarea per la visualizzazione cronologica //
  ricezione.textContent += messaggioRicevuto.data + "\n"; 

  // Salva il contenuto del messaggio ricevuto per eventuali controlli //
  let data = messaggioRicevuto.data;
  console.log(data); 

  // Se il messaggio è "login errato", mostra un alert //
  if (data === "rlo|login errato") {
    alert("Login errato! Riprova.");
  } 

  // Altrimenti, mostra l’area chat e nasconde l’area login //
  else {
    document.getElementById("ricezione").textContent += data + "\n";
    document.getElementById("loginArea").style.display = "none";
    document.getElementById("chatArea").style.display = "block";
  }
} 

  // Legge il messaggio scritto dall’utente //
function inviaAlServer() {
  let messaggioDaInviare = document.getElementById("msg").value; 

  // Prende il nome utente per includerlo nel messaggio //
  let user = document.getElementById("nome").value; 

  // Genera un timestamp ISO per marcare il momento del messaggio //
  let timestamp = new Date().toISOString(); 

  // Costruisce un messaggio formattato "msg|utente|timestamp|messaggio" //
  let mes = "msg|" + user + "|" + timestamp + "|" + messaggioDaInviare; 

  // Invia il messaggio al server tramite WebSocket //
  ws.send(mes); 

  // Svuota il campo di input del messaggio dopo l’invio //
  document.getElementById("msg").value = "";
} 

  // Chiude la connessione WebSocket con il server //
function chiudiLaConnessione() {
  ws.close(); 

  // Nasconde l’area chat e mostra quella di login per permettere nuovo accesso //
  document.getElementById("chatArea").style.display = "none";
  document.getElementById("loginArea").style.display = "block";
} 
