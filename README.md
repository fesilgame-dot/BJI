<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>বাংলাদেশ জামায়াতে ইসলামী</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<style>
body{font-family:'Roboto',sans-serif;margin:0;padding:0;background:#f0f2f5;}
header{display:flex;justify-content:center;align-items:center;gap:10px;padding:12px;background:#1e3799;color:white;flex-wrap:wrap;position:relative;}
header img{max-width:50px;border-radius:6px;}
header .org-info{flex:1;text-align:center;}
header h1{margin:0;font-size:18px;}
header p{margin:0;font-size:12px;}
header .settings-btn{position:absolute;right:15px;top:12px;font-size:20px;cursor:pointer;color:white;}
#dashboard{display:grid;grid-template-columns:repeat(auto-fit,minmax(120px,1fr));gap:10px;margin:10px;}
#dashboard button{background:#0097e6;color:white;border:none;border-radius:10px;padding:12px;font-size:13px;font-weight:bold;cursor:pointer;display:flex;flex-direction:column;align-items:center;gap:5px;transition:0.3s;}
#dashboard button i{font-size:18px;}
#dashboard button:hover{background:#0652dd;}
.form-section{display:none;background:white;margin:10px auto;padding:12px;border-radius:12px;max-width:95%;box-shadow:0 3px 10px rgba(0,0,0,0.1);}
.form-section h2{text-align:center;margin-bottom:10px;color:#1e3799;font-size:16px;}
.input-group{display:flex;align-items:center;border:1px solid #ccc;border-radius:8px;padding:5px;margin:5px 0;}
.input-group i{margin-right:6px;color:#1e3799;font-size:13px;}
.input-group input,.input-group select{border:none;outline:none;flex:1;font-size:13px;}
button.action-btn{margin-top:6px;padding:9px;width:100%;background:#0097e6;color:white;border:none;border-radius:8px;cursor:pointer;font-size:13px;font-weight:bold;}
button.action-btn:hover{background:#0652dd;}
table{width:100%;border-collapse:collapse;margin-top:10px;font-size:12px;text-align:center;}
th,td{border:1px solid #ddd;padding:5px;}
th{background:#0097e6;color:white;}
td button{margin:1px;padding:3px 5px;font-size:11px;cursor:pointer;border-radius:5px;border:none;background:#1e3799;color:white;}
td button:hover{background:#0652dd;}
.slip-card{border:1px solid #ddd;border-radius:10px;padding:6px;margin:5px 0;background:#fff;box-shadow:0 2px 5px rgba(0,0,0,0.1);font-size:12px;text-align:left;position:relative;max-width:300px;}
.slip-card img{max-width:35px;margin:0 auto 3px;display:block;}
.slip-card h3{text-align:center;margin:0;font-size:13px;color:#1e3799;}
.slip-card p{margin:2px 0;font-size:12px;display:flex;align-items:center;gap:4px;}
.slip-card small{display:block;margin-top:2px;color:#555;font-style:italic;font-size:11px;text-align:center;}
.summary{background:#fff;padding:10px;margin:10px 0;border-radius:10px;box-shadow:0 2px 5px rgba(0,0,0,0.1);font-size:14px;}
@media(max-width:600px){#dashboard{grid-template-columns:repeat(2,1fr);}table,thead,tbody,th,td,tr{display:block;width:100%;}th,td{text-align:right;padding-left:50%;position:relative;}th::before,td::before{position:absolute;left:10px;width:45%;white-space:nowrap;text-align:left;font-weight:bold;}}
</style>
</head>
<body>

<header>
<img id="orgLogo" src="logo.png" alt="সংস্থা লোগো">
<div class="org-info">
<h1 id="orgName">সংস্থা নাম</h1>
<p id="orgAddress">ঠিকানা: ঢাকা</p>
</div>
<i class="fa-solid fa-gear settings-btn" onclick="showSection('settings')"></i>
</header>

<div id="dashboard">
<button onclick="showSection('newMember')"><i class="fa-solid fa-user-plus"></i> নতুন সদস্য</button>
<button onclick="showSection('allMembers')"><i class="fa-solid fa-users"></i> সদস্য তালিকা</button>
<button onclick="showSection('newPayment')"><i class="fa-solid fa-hand-holding-dollar"></i> নতুন জমা</button>
<button onclick="showSection('allPayments')"><i class="fa-solid fa-list"></i> জমা তালিকা</button>
<button onclick="showSection('summary')"><i class="fa-solid fa-chart-simple"></i> মোট হিসাব</button>
</div>

<!-- নতুন সদস্য -->
<div id="newMember" class="form-section">
<h2>নতুন সদস্য</h2>
<div class="input-group"><i class="fa-solid fa-user"></i><input type="text" id="memberName" placeholder="নাম"></div>
<div class="input-group"><i class="fa-solid fa-layer-group"></i><input type="text" id="memberType" placeholder="ধরন"></div>
<div class="input-group"><i class="fa-solid fa-phone"></i><input type="text" id="memberMobile" placeholder="মোবাইল"></div>
<div class="input-group"><i class="fa-solid fa-location-dot"></i><input type="text" id="memberAddress" placeholder="ঠিকানা"></div>
<button class="action-btn" onclick="addMember()">যোগ করুন</button>
</div>

<!-- সদস্য তালিকা -->
<div id="allMembers" class="form-section">
<h2>সদস্য তালিকা</h2>
<table id="membersTable">
<thead><tr><th>নাম</th><th>ধরন</th><th>মোবাইল</th><th>ঠিকানা</th><th>একশন</th></tr></thead>
<tbody></tbody>
</table>
<button class="action-btn" onclick="exportMembersExcel()">Excel Export</button>
<button class="action-btn" onclick="clearMembers()">সকল সদস্য ডিলেট</button>
</div>

<!-- নতুন জমা -->
<div id="newPayment" class="form-section">
<h2>নতুন জমা</h2>
<div class="input-group"><i class="fa-solid fa-user"></i><select id="paymentMember"></select></div>
<div class="input-group"><i class="fa-solid fa-sack-dollar"></i><input type="number" id="paymentAmount" placeholder="টাকা"></div>
<div class="input-group"><i class="fa-solid fa-comment"></i><input type="text" id="paymentComment" placeholder="মন্তব্য"></div>
<div class="input-group"><i class="fa-solid fa-user-tie"></i><input type="text" id="collectorName" placeholder="গ্রহণকারী"></div>
<button class="action-btn" onclick="addPayment()">সংরক্ষণ করুন</button>
</div>

<!-- জমা তালিকা -->
<div id="allPayments" class="form-section">
<h2>জমা তালিকা</h2>
<div>
<button class="action-btn" onclick="exportPaymentsExcel()">Excel Export</button>
<button class="action-btn" onclick="clearPayments()">সকল জমা ডিলেট</button>
</div>
<div id="paymentsReport"></div>
</div>

<!-- summary -->
<div id="summary" class="form-section">
<h2>মোট হিসাব</h2>
<div class="summary">
<p>👥 মোট সদস্য: <span id="totalMembers">0</span></p>
<p>💰 মোট জমা: ৳<span id="totalAmount">0</span></p>
<p>📌 সর্বশেষ জমা: <span id="lastPayment">-</span></p>
</div>
<div id="memberWise"></div>
<button class="action-btn" onclick="downloadSummaryExcel()">মোট হিসাব Excel ডাউনলোড</button>
</div>

<!-- settings -->
<div id="settings" class="form-section">
<h2>সেটিংস</h2>
<div class="input-group"><i class="fa-solid fa-building"></i><input type="text" id="setOrgName" placeholder="সংস্থা নাম"></div>
<div class="input-group"><i class="fa-solid fa-map-marker-alt"></i><input type="text" id="setOrgAddress" placeholder="ঠিকানা"></div>
<div class="input-group"><i class="fa-solid fa-user-tie"></i><input type="text" id="setDefaultCollector" placeholder="ডিফল্ট সংগ্রাহক"></div>
<div class="input-group"><i class="fa-solid fa-comment-dots"></i><input type="text" id="setThanksNote" placeholder="স্লিপে ধন্যবাদ নোট"></div>
<div class="input-group"><i class="fa-solid fa-image"></i><input type="file" id="setOrgLogo" accept="image/*"></div>
<button class="action-btn" onclick="saveSettings()">সংরক্ষণ করুন</button>
</div>

<script>
// IndexedDB setup
let db; let settings={};
let request=indexedDB.open("MyDatabase",1);
request.onupgradeneeded=e=>{
  db=e.target.result;
  if(!db.objectStoreNames.contains("members")) db.createObjectStore("members",{keyPath:"id"});
  if(!db.objectStoreNames.contains("payments")) db.createObjectStore("payments",{keyPath:"id"});
  if(!db.objectStoreNames.contains("settings")) db.createObjectStore("settings",{keyPath:"key"});
};
request.onsuccess=e=>{
  db=e.target.result;
  showSection("newMember"); loadPaymentMembers(); loadSettings(); loadSummary(); loadMembers(); loadPayments();
};

// IndexedDB helpers
function getAll(store,callback){ let tx=db.transaction(store,"readonly"); let os=tx.objectStore(store); let res=[]; os.openCursor().onsuccess=e=>{ let cursor=e.target.result; if(cursor){ res.push(cursor.value); cursor.continue(); } else callback(res); } }
function save(store,obj,callback){ let tx=db.transaction(store,"readwrite"); let os=tx.objectStore(store); os.put(obj); tx.oncomplete=()=>callback&&callback(); }
function remove(store,key,callback){ let tx=db.transaction(store,"readwrite"); let os=tx.objectStore(store); os.delete(key); tx.oncomplete=()=>callback&&callback(); }

// Section switch
function showSection(id){ document.querySelectorAll(".form-section").forEach(s=>s.style.display="none"); document.getElementById(id).style.display="block"; if(id=="allMembers") loadMembers(); if(id=="allPayments") loadPayments(); if(id=="newPayment") loadPaymentMembers(); if(id=="summary") loadSummary(); if(id=="settings") loadSettings(); }

// Member CRUD
function addMember(){ let name=document.getElementById("memberName").value; if(!name) return alert("নাম দিন"); let member={id:Date.now(),name:name,type:document.getElementById("memberType").value||"",mobile:document.getElementById("memberMobile").value||"",address:document.getElementById("memberAddress").value||""}; save("members",member,()=>{ document.getElementById("memberName").value=""; document.getElementById("memberType").value=""; document.getElementById("memberMobile").value=""; document.getElementById("memberAddress").value=""; loadMembers(); loadPaymentMembers(); loadSummary(); }); }

function loadMembers(){ getAll("members",members=>{ let tbody=document.querySelector("#membersTable tbody"); tbody.innerHTML=""; members.forEach(m=>{ let tr=document.createElement("tr"); tr.innerHTML=`<td>${m.name}</td><td>${m.type}</td><td>${m.mobile}</td><td>${m.address}</td><td><button onclick="editMember(${m.id})">✏️</button><button onclick="deleteMember(${m.id})">🗑️</button></td>`; tbody.appendChild(tr); }); }); }
function editMember(id){ getAll("members",members=>{ let m=members.find(x=>x.id==id); if(!m)return; document.getElementById("memberName").value=m.name; document.getElementById("memberType").value=m.type; document.getElementById("memberMobile").value=m.mobile; document.getElementById("memberAddress").value=m.address; remove("members",id,()=>{ loadMembers(); loadPaymentMembers(); showSection("newMember"); }); }); }
function deleteMember(id){ remove("members",id,()=>{ loadMembers(); loadPaymentMembers(); loadSummary(); }); }
function clearMembers(){ if(confirm("আপনি কি সকল সদস্য ডিলেট করতে চান?")){ getAll("members",members=>{ members.forEach(m=>remove("members",m.id)); loadMembers(); loadPaymentMembers(); loadSummary(); }); } }

// Payment CRUD
function addPayment(){ let member=document.getElementById("paymentMember").value; let amount=document.getElementById("paymentAmount").value; if(!member||!amount) return alert("সব ঘর পূরণ করুন"); let payment={id:Date.now(),member,amount,comment:document.getElementById("paymentComment").value||"",collector:document.getElementById("collectorName").value||"",dateTime:new Date().toLocaleString()}; save("payments",payment,()=>{ document.getElementById("paymentAmount").value=""; document.getElementById("paymentComment").value=""; document.getElementById("collectorName").value=""; loadPayments(); loadSummary(); }); }
function loadPaymentMembers(){ getAll("members",members=>{ let sel=document.getElementById("paymentMember"); sel.innerHTML=""; members.forEach(m=>{ let opt=document.createElement("option"); opt.text=m.name; sel.add(opt); }); }); }
function deletePayment(id){ remove("payments",id,()=>{ loadPayments(); loadSummary(); }); }
function clearPayments(){ if(confirm("আপনি কি সকল জমা ডিলেট করতে চান?")){ getAll("payments",payments=>{ payments.forEach(p=>remove("payments",p.id)); loadPayments(); loadSummary(); }); } }

// Load payments & render slips
function loadPayments(){ getAll("payments",payments=>{ let div=document.getElementById("paymentsReport"); div.innerHTML=""; payments.forEach((p,index)=>{ let slip=document.createElement("div"); slip.className="slip-card"; slip.id="slip-"+p.id; slip.innerHTML=`<p style="text-align:center;font-weight:bold;">স্লিপ নং: ${index+1}</p>${settings.logo?`<img src="${settings.logo}">`:''}<h3>${settings.orgName||'সংস্থা'}</h3><small>${settings.orgAddress||''}</small><p>👤 সদস্য: ${p.member}</p><p>📅 তারিখ: ${p.dateTime}</p><p>💰 পরিমাণ: ৳${p.amount}</p><p>📝 মন্তব্য: ${p.comment}</p><p>✅ গ্রহণকারী: ${p.collector||settings.defaultCollector||''}</p><small>${settings.thanksNote||''}</small><div style="display:flex;gap:5px;margin-top:5px;"><button onclick="downloadSlip(${p.id})">⬇️ ডাউনলোড</button><button onclick="shareSlip(${p.id})">📤 শেয়ার</button><button onclick="deletePayment(${p.id})">🗑️ ডিলেট</button></div>`; div.appendChild(slip); }); }); }

// Download & Share slip
function downloadSlip(id){ let slip=document.getElementById("slip-"+id); slip.style.background="#fff"; html2canvas(slip).then(canvas=>{ let link=document.createElement("a"); link.download="slip_"+id+".png"; link.href=canvas.toDataURL("image/png"); link.click(); }); }
function shareSlip(id){ let slip=document.getElementById("slip-"+id); slip.style.background="#fff"; html2canvas(slip).then(canvas=>{ canvas.toBlob(blob=>{ const file=new File([blob],`slip_${id}.png`,{type:"image/png"}); if(navigator.canShare && navigator.canShare({files:[file]})){ navigator.share({files:[file],title:"সদস্য স্লিপ",text:"সদস্যের জমা তথ্য"}).catch(err=>alert("শেয়ার করা যায়নি: "+err)); } else alert("আপনার ব্রাউজার শেয়ার সাপোর্ট করে না। ডাউনলোড করুন।"); }); }); }

// Summary
function loadSummary(){ getAll("members",members=>{ document.getElementById("totalMembers").innerText=members.length; }); getAll("payments",payments=>{ let total=payments.reduce((a,b)=>a+Number(b.amount),0); document.getElementById("totalAmount").innerText=total; document.getElementById("lastPayment").innerText=payments.length?payments[payments.length-1].dateTime:'-'; let memberDiv=document.getElementById("memberWise"); memberDiv.innerHTML="<h4>সদস্য অনুযায়ী জমা:</h4>"; let sums={}; payments.forEach(p=>{ sums[p.member]=(sums[p.member]||0)+Number(p.amount); }); for(let m in sums){ memberDiv.innerHTML+=`<p>${m}: ৳${sums[m]}</p>`; } }); }

// Excel Export
function exportMembersExcel(){ getAll("members",members=>{ let wb=XLSX.utils.book_new(); let ws=XLSX.utils.json_to_sheet(members); XLSX.utils.book_append_sheet(wb,ws,"Members"); XLSX.writeFile(wb,"members.xlsx"); }); }
function exportPaymentsExcel(){ getAll("payments",payments=>{ let wb=XLSX.utils.book_new(); let ws=XLSX.utils.json_to_sheet(payments); XLSX.utils.book_append_sheet(wb,ws,"Payments"); XLSX.writeFile(wb,"payments.xlsx"); }); }
function downloadSummaryExcel(){ getAll("members",members=>{ getAll("payments",payments=>{ let sums={}; payments.forEach(p=>{ sums[p.member]=(sums[p.member]||0)+Number(p.amount); }); let data=[]; members.forEach(m=>{ data.push({Name:m.name,Type:m.type,Mobile:m.mobile,Address:m.address,TotalAmount:sums[m.name]||0}); }); let wb=XLSX.utils.book_new(); let ws=XLSX.utils.json_to_sheet(data); XLSX.utils.book_append_sheet(wb,ws,"Summary"); XLSX.writeFile(wb,"summary.xlsx"); }); }); }

// Settings
function saveSettings(){ settings.orgName=document.getElementById("setOrgName").value; settings.orgAddress=document.getElementById("setOrgAddress").value; settings.defaultCollector=document.getElementById("setDefaultCollector").value; settings.thanksNote=document.getElementById("setThanksNote").value; let f=document.getElementById("setOrgLogo").files[0]; if(f){ let r=new FileReader(); r.onload=e=>{ settings.logo=e.target.result; save("settings",{key:"config",value:settings},loadSettings); }; r.readAsDataURL(f); } else { save("settings",{key:"config",value:settings},loadSettings); } }
function loadSettings(){ getAll("settings",arr=>{ let s=arr.find(x=>x.key=="config"); if(s){ settings=s.value; document.getElementById("orgName").innerText=settings.orgName||"সংস্থা"; document.getElementById("orgAddress").innerText=settings.orgAddress||""; if(settings.logo) document.getElementById("orgLogo").src=settings.logo; } }); }
</script>
</body>
</html>
