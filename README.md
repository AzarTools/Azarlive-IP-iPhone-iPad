# 🔍 Azar IP Scanner - iPhone/iPad

Un bookmarklet simple pour détecter les adresses IP sur [Azar](https://azarlive.com/) Live avec géolocalisation.

---

## ✨ Fonctionnalités

- 🎯 Détection automatique d'IP via WebRTC
- 🌍 Géolocalisation (Ville, Pays, ISP)
- 📱 Interface mobile déplaçable
- 📋 Copie d'IP en un clic

---

## 🚀 Installation sur Safari (iPhone/iPad)

1. Ouvrez **Safari** et allez sur [azarlive.com](https://azarlive.com/)

2. Créez un signet :
   - Appuyez sur les trois petit points en bas à droite 
   - Sélectionnez **"Ajouter un signet"**
   - Appuyez sur **"Enregistrer"**

3. Modifiez le signet :
   - Appuyez sur l'icône des **signets** (📖)
   - Restez appuyé sur le signet créé
   - Sélectionnez **"Modifier"**

4. Remplacez le contenu :
   - (Optionnel) : **Nom** : `IP Tracker`
   - (Obligatoire) : **URL** : Supprimez tout et collez le code ci-dessous
   - Appuyez sur **"OK"**

### Code à copier :

```javascript
javascript:(function(){'use strict';const CORS_PROXY='https://corsproxy.io/?';const APIS=[{url:(ip)=>`${CORS_PROXY}http://ip-api.com/json/${ip}`,parse:d=>({city:d.city,region:d.regionName,country:d.country,isp:d.isp,vpn:false})},{url:(ip)=>`${CORS_PROXY}https://ipwhois.app/json/${ip}`,parse:d=>({city:d.city,region:d.region,country:d.country,isp:d.isp,vpn:d.security?.vpn||d.security?.proxy||d.security?.tor||false})}];const container=document.createElement('div');container.id='ip-mobile';container.innerHTML=`<div id="drag-handle" style="cursor:move;padding:8px;background:#51f59b;margin:-10px -10px 10px -10px;border-radius:8px 8px 0 0;display:flex;justify-content:space-between;align-items:center;"><span style="font-weight:800;font-size:12px;color:#121212;">📍 IP TRACKER</span><button id="close-ip" style="padding:4px 8px;border:none;background:#121212;color:#51f59b;border-radius:4px;font-weight:700;font-size:12px;">✕</button></div><div id="ip-list" style="font-size:11px;"></div>`;Object.assign(container.style,{position:'fixed',top:'80px',right:'10px',width:'200px',maxHeight:'250px',backgroundColor:'#121212',border:'2px solid #51f59b',borderRadius:'10px',padding:'10px',zIndex:'99999',fontFamily:'Arial,sans-serif',boxShadow:'0 4px 15px rgba(81,245,155,0.4)',color:'#fff',overflow:'auto',touchAction:'none'});document.body.appendChild(container);document.getElementById('close-ip').onclick=()=>container.remove();let pos={x:0,y:0,mx:0,my:0};const handle=document.getElementById('drag-handle');handle.ontouchstart=handle.onmousedown=(e)=>{e.preventDefault();const evt=e.touches?e.touches[0]:e;pos.mx=evt.clientX;pos.my=evt.clientY;const move=(e)=>{const evt=e.touches?e.touches[0]:e;pos.x=pos.mx-evt.clientX;pos.y=pos.my-evt.clientY;pos.mx=evt.clientX;pos.my=evt.clientY;container.style.top=(container.offsetTop-pos.y)+'px';container.style.left=(container.offsetLeft-pos.x)+'px';container.style.right='auto';};const stop=()=>{document.removeEventListener('mousemove',move);document.removeEventListener('mouseup',stop);document.removeEventListener('touchmove',move);document.removeEventListener('touchend',stop);};document.addEventListener('mousemove',move);document.addEventListener('mouseup',stop);document.addEventListener('touchmove',move);document.addEventListener('touchend',stop);};const fetchIPInfo=async(ip)=>{for(const api of APIS){try{const res=await fetch(api.url(ip));const data=await res.json();if(data&&!data.error&&data.status!=='fail'){return api.parse(data);}}catch(e){continue;}}return null;};let detectedIP=null;window.oRTCPeerConnection=window.oRTCPeerConnection||window.RTCPeerConnection;window.RTCPeerConnection=function(...args){const pc=new window.oRTCPeerConnection(...args);pc.oaddIceCandidate=pc.addIceCandidate;pc.addIceCandidate=async function(iceCandidate,...rest){if(iceCandidate?.candidate){const fields=iceCandidate.candidate.split(' ');if(fields[7]==='srflx'){const ip=fields[4];if(detectedIP!==ip){detectedIP=ip;const ipList=document.getElementById('ip-list');ipList.innerHTML='<div style="text-align:center;color:#51f59b;padding:8px;font-size:10px;">🔍 Scan...</div>';const geoData=await fetchIPInfo(ip);const{city='?',country='?',isp='N/A',vpn=false}=geoData||{};const vpnTag=vpn?'<span style="background:#ff4444;color:#fff;padding:1px 4px;border-radius:3px;font-size:9px;margin-left:3px;">VPN</span>':'';ipList.innerHTML=`<div style="background:#1a1a1a;border-left:2px solid #51f59b;padding:8px;border-radius:6px;margin-bottom:8px;"><div style="margin-bottom:4px;font-size:12px;font-weight:700;color:#51f59b;word-break:break-all;">${ip}${vpnTag}</div><div style="margin-bottom:3px;font-size:10px;">📍 ${city}, ${country}</div><div style="margin-bottom:8px;font-size:10px;opacity:0.8;">📡 ${isp}</div><button onclick="navigator.clipboard.writeText('${ip}');this.innerText='✓';setTimeout(()=>this.innerText='📋 Copier',1000)" style="width:100%;padding:8px;border:none;background:#51f59b;color:#121212;border-radius:6px;font-weight:700;font-size:11px;">📋 Copier</button></div>`;}}}return pc.oaddIceCandidate(iceCandidate,...rest);};return pc;};})();
```

---

## 📖 Utilisation

1. Allez sur [azarlive.com](https://azarlive.com/)
2. Appuyez sur l'icône des **signets** (📖)
3. Sélectionnez **"IP Tracker"**
4. Lancez un appel
5. L'IP s'affiche automatiquement !

**Contrôles :**
- **Glissez la barre verte** pour déplacer l'interface
- **Bouton ✕** pour fermer
- **Bouton 📋 Copier** pour copier l'IP

---


## ⚖️ Avertissement

Ce script est fourni **à des fins éducatives uniquement**.
- Respectez les lois sur la vie privée
- L'utilisation peut violer les conditions d'Azar

---

<div align="center">
Made with ❤️ by VeltrixJS
</div>
