# GroupeSP-PWA
SP PWA Application web
CODE SOURCE

**********************INDEX.HTML*******************************
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" crossorigin="anonymous">
    <link rel="stylesheet" href="css/bootstrap.min.css">
    <link rel="stylesheet" href="css/style.css">
    <Link rel="manifest" href="js/manifest.json">
    <title>Appli Chat </title>
    <link name="theme-color" content=white"">
    <link rel="shortcut icon" href="/images/icons/icon-128x128.png">
</head>
<body>
<div class="container-fluid">
    <header>
            <h1> NOFCOL-CHAT </h1>
            <button id="install_button" hidden>Load AC</button>
    </header>   
</div>
<main class="container">
  <div class="app"></div>
  <div class="rows">
   <div class="col-sm-4"> 
    <img src="images/profil.png" alt="photo" class="rounded-circle" class="img2">
    <p class="nomuser"> Redd Saint Clair </p>
  </div>
 <form id="form">
<div class="col-sm-8">
    <div class="mb-3">
        <label for="nom" class="form-label">Prenom</label>
        <input type="text" name="nom" class="form-control" id="nom">
      </div>
      <div class="mb-3 has-error">
        <label for="fichier" class="form-label">Joindre</label>
        <input class="form-control" type="file" id="fichier">
      </div>
      <div class="mb-3 has-error">
        <label for="message" class="form-label">Message</label>
        <textarea class="form-control" id="message" rows="3"></textarea>
      </div>
</div>
<button type="submit" class="btn btn-primary">Envoyez</button>
</form>
</div>
</div>
</div>      
</main>
<footer>
<img src="/images/nofcolnum.jpg" alt="logo" class="img3">
</footer>
<script> 

  if("serviceWorker" in navigator) {
  navigator.serviceWorker.register('sw.js');
}
if(window.Notification && window.Notification !== 'denied'){
    Notification.requestPermission(perm =>{
      if(perm === 'granted'){
          const notif = new Notification ('my firt notification');
      } else { 
        consol.log('No reception?')
      }
    })
  }
</script>
<script src="sw.js"></script>
<script src="./js/index.js" defer></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" crossorigin="anonymous"></script> 
</body>
</html>


************************ STYLE.CSS ************************************

*{
    margin: 0px;
    padding: 0px;
    box-sizing: border-box;
}
body
{
    font-family: Georgia, 'Times New Roman', Times, serif;
    justify-content: center;  
}
.container-fluid, footer
{
    min-height: 10vh;
    background-color: rgb(3, 90, 78);
    border: 3px solid rgb(243, 239, 239);
    padding-left: 0;
    padding-right: 0;
}
h1{
    padding: 10px;
    text-align: center; 
    color: rgb(253, 253, 251); 
}
.col-sm-4 img
{
    width: 17%;
    background-color: beige;
    padding: 3px;
}
p{
    float: right;
    padding-top: 10px;
    margin: 5px;
}
img
{
margin-left: 170px;
}
.img3
{
border-radius: 50px;
width: 60px;
padding-top: 1px;
}
#form{
margin: 0.5px;
padding: 50px;
background-color: beige;
border-radius: 40px;
}
textarea{
    border-radius: 100px;
}
main
{
    min-height: 70vh;
    background-color: rgb(186, 190, 163);
}
.container{
 padding:  30px;
 margin: auto;
 border: solid 1px rgb(63, 61, 61);
}
#install_button{
    margin-left: auto;
    padding: 10px;
    border-radius: 20px;
    background-color: brown;
    color: blanchedalmond;
    float: right;
}

@media screen and (max-width:900px){
main{
    width: 95%;

}

}

******************** SW.JS  ************************

const acNofcol = "ACLPDWCA";
//var cacheBlacklist = "acNofcol"; 

 // Mise en cache
self.addEventListener('install', event =>{
   event.waitUntil(
    caches.open('acNofcol').then((cache) => {
       // console.log('Opened cache')
         cache.addAll([
            'index.html',
            'sw.js'
        ])
    })
   )
})

self.addEventListener('activate', event => {
    event.waitUntil(
      caches.keys().then( (keys) => {
        return Promise.add(keys
            .filter((keys) => keys !== acNofcol)
            .map((key)  => caches.delete(key))
             );
          })
        );
      });

self.addEventListener("fetch", (event) => { 

    if(!(event.request.url.indexOf('http')=== 0)) return;

    event.respondWith( caches.match(event.request)
        .then((rep) => {
          if (rep) {
            return rep;
        }
        //var fetchRequest = event.request.clone();
          return fetch(event.request).then(newRep => {
              //if (!response || response.status !== 200 || response.type !== 'basic') {
               // return response;
              //}
             
            //var responseToCache = response.clone();
              caches.open(acNofcol)
                .then((cache) => cache.put(event.request, newRep)
                );
              return newRep.clone();           
             }
          )
        })
    )
  })
  
************** MANIFEST.JS ********************
{
    "name": "APPLI CHAT",
    "short_name": "AC",
    "start_url" : "/index.html",
    "display": "standalone",
    "theme_color": "black",
    "background_color": "black",
    "description" : "application-Chat",
    "orientation": "portrait-primary",
    "icons": [
      {
        "src": "/images/icons/icon-72x72.png",
        "type": "image/png",
        "sizes": "72x72"
      },
      {
        "src": "/images/icons/icon-96x96.png",
        "type": "image/png",
        "sizes": "96x96"
      },
      {
        "src": "/images/icons/icon-128x128.png",
        "type": "image/png",
        "sizes": "128x128"
      },
      {
        "src": "/images/icons/icon-144x144.png",
        "type": "image/png",
        "sizes": "144x144"
      },
      {
        "src": "/images/icons/icon-152x152.png",
        "type": "image/png",
        "sizes": "152x152"
      },
      {
        "src": "/images/icons/icon-192x192.png",
        "type": "image/png",
        "sizes": "192x192"
      },
      {
        "src": "/images/icons/icon-512x512.png",
        "type": "image/png",
        "sizes": "512x512"
      }
    ] 
  }
************* INDEX.JS *********************

let form = document.getElementById('form');
let prenom = document.getElementById('nom');
let message = document.getElementById('message');
let joindre = document.getElementById('fichier');
let app = document.getElementById('app');


//let ExistingItems = localStorage.getItem('items');
//if(ExistingItems){
//  ExistingItems = JSON.parse(ExistingItems);
//    ExistingItems.forEach(item => {
//      let p = document.createElement('p');
//    p.innerHTML = item.prenom +':'+ item.message;3
//       appli.appendChild(p);
//    });
//}
form.addEventListener('submit', (e)=>{
    e.preventDefault();
if(prenom.value ==""){
    alert("veuillez saisir votre prenom");
}
else if(message.value == ""){
    alert("veuillez saisir un message");
}

let item = {
    id:Math.floor(Date.now() / 1000),
    prenom:prenom.value, 
    message:message.value
}
let items = [];

if(localStorage.getItem('items')){
    let lItems = JSON.parse(localStorage.getItem('items'));
    items = lItems;
}

items.push(item)
localStorage.setItem('items', JSON.stringify(items)); 
})

let deferredPrompt;
const installButton = document.getElementById("install_button");

window.addEventListener("beforeinstallprompt", e =>{
    console.log("beforeinstallprompt fired"); 
    e.preventDefault();
    deferredPrompt = e;

    installButton.hidden = false;
    installButton.addEventListener("click", installApp);
});

function installApp() {
    deferredPrompt.prompt();
    installButton.disabled = true; 

    deferredPrompt.userChoice.then(choiceResult =>{
        if (choiceResult.outcome == "accepter") {
            console.log("AC setup accepter");
            installButton.hidden = true;
        } else{
            console.log("AC setup rejeter")
        }
        installButton.disabled = false;
        deferredPrompt = null;
    });
}
