
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<meta name="theme-color" content="#6c4bd8">
<title>مذكّراتي | My Diary</title>
<style>
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
body{
 margin:0;
 font-family:Tahoma,Arial,sans-serif;
 background:#f5f2ff;
 color:#29243b;
 transition:.3s;
}
body.dark{background:#171522;color:#fff}
#app{max-width:750px;margin:auto;min-height:100vh;padding:15px}
header{
 display:flex;align-items:center;justify-content:space-between;
 padding:12px 4px;
}
.logo{font-size:25px;font-weight:900;color:#7955dc}
.iconbtn{
 border:0;background:#fff;border-radius:14px;
 padding:11px;font-size:20px;cursor:pointer;
}
.dark .iconbtn{background:#302b43;color:white}
.welcome{
 background:linear-gradient(135deg,#805de6,#b18cf8);
 color:white;padding:23px;border-radius:25px;
 box-shadow:0 8px 20px #805de633;
}
.welcome h2{margin:0 0 10px;font-size:23px}
.welcome p{margin:0;opacity:.9}
.search{
 width:100%;padding:15px;border:0;border-radius:17px;
 margin:18px 0;background:white;font-size:16px;outline:none;
}
.dark .search,.dark textarea,.dark input{background:#302b43;color:white}
.actions{display:flex;gap:10px;margin-bottom:18px}
.action{
 flex:1;border:0;border-radius:16px;padding:14px;
 background:#fff;color:#6746ce;font-weight:bold;font-size:15px;
}
.dark .action{background:#302b43;color:white}
.notes{display:grid;gap:12px}
.note{
 background:white;padding:17px;border-radius:20px;
 box-shadow:0 4px 12px #00000009;
 border-right:5px solid #9575e8;
}
.dark .note{background:#252235}
.note h3{margin:0 0 9px;font-size:18px}
.note p{margin:0;line-height:1.8;white-space:pre-wrap;overflow-wrap:anywhere}
.date{font-size:12px;color:#999;margin-top:12px}
.note-buttons{display:flex;gap:8px;margin-top:12px}
.note-buttons button{
 border:0;border-radius:10px;padding:8px 12px;
 background:#f0ebff;color:#6848c9;font-size:14px;
}
.dark .note-buttons button{background:#39314e;color:white}
.fab{
 position:fixed;bottom:25px;left:25px;
 width:62px;height:62px;border:0;border-radius:50%;
 background:#7955dc;color:white;font-size:32px;
 box-shadow:0 6px 18px #7955dc66;
}
.modal{
 display:none;position:fixed;inset:0;background:#0008;
 align-items:center;justify-content:center;padding:15px;z-index:5;
}
.modal.show{display:flex}
.panel{
 background:#fff;border-radius:25px;padding:20px;
 width:100%;max-width:500px;max-height:90vh;overflow:auto;
}
.dark .panel{background:#252235}
.panel h2{margin-top:0}
.panel input,.panel textarea{
 width:100%;padding:14px;border:1px solid #ddd;
 border-radius:13px;font-size:16px;margin-bottom:12px;
 font-family:inherit;
}
.panel textarea{height:180px;resize:vertical;line-height:1.7}
.primary{
 width:100%;border:0;border-radius:14px;padding:15px;
 background:#7955dc;color:white;font-size:17px;font-weight:bold;
}
.secondary{
 width:100%;border:0;border-radius:14px;padding:13px;
 margin-top:8px;background:#eee;color:#333;font-size:15px;
}
.dark .secondary{background:#39314e;color:white}
.empty{text-align:center;padding:45px 10px;color:#999}
#lockScreen{
 display:none;position:fixed;inset:0;background:#7955dc;
 z-index:10;color:white;align-items:center;justify-content:center;
 text-align:center;padding:25px;
}
#lockScreen input{
 padding:15px;border:0;border-radius:12px;
 font-size:22px;text-align:center;width:220px;
}
</style>
</head>
<body>
<div id="app">
<header>
 <div class="logo">📔 مذكّراتي</div>
 <button class="iconbtn" onclick="toggleDark()">🌙</button>
</header>

<section class="welcome">
 <h2>أهلاً بك في عالمك الخاص 💜</h2>
 <p id="today"></p>
 <p>اكتب أفكارك، احفظ ذكرياتك، وعبّر عن نفسك.</p>
</section>

<input class="search" id="search" placeholder="🔍 ابحث في مذكراتك..." oninput="render()">

<div class="actions">
 <button class="action" onclick="filter='all';render()">📚 الكل</button>
 <button class="action" onclick="filter='fav';render()">❤️ المفضلة</button>
 <button class="action" onclick="exportNotes()">📥 نسخة احتياطية</button>
</div>

<div class="notes" id="notes"></div>

<button class="fab" onclick="openEditor()">+</button>
</div>

<div class="modal" id="editor">
 <div class="panel">
  <h2 id="editorTitle">مذكرة جديدة ✍️</h2>
  <input id="noteTitle" placeholder="عنوان المذكرة">
  <textarea id="noteText" placeholder="اكتب ما يدور في بالك..."></textarea>
  <button class="primary" onclick="saveNote()">💾 حفظ المذكرة</button>
  <button class="secondary" onclick="closeEditor()">إلغاء</button>
 </div>
</div>

<div class="modal" id="settings">
 <div class="panel">
  <h2>🔐 إعدادات الخصوصية</h2>
  <p>أنشئ رمزاً سرياً لقفل المذكرات.</p>
  <input id="pinInput" type="password" inputmode="numeric" maxlength="8" placeholder="رمز سري جديد">
  <button class="primary" onclick="setPin()">حفظ الرمز</button>
  <button class="secondary" onclick="removePin()">إزالة القفل</button>
  <button class="secondary" onclick="closeSettings()">إغلاق</button>
 </div>
</div>

<div id="lockScreen">
 <div>
  <h1>🔒 مذكّراتي</h1>
  <p>أدخل الرمز السري لفتح مذكراتك</p>
  <input id="unlockPin" type="password" inputmode="numeric" placeholder="الرمز السري">
  <br><br>
  <button class="primary" onclick="unlock()">فتح التطبيق</button>
 </div>
</div>

<script>
const $=id=>document.getElementById(id);
let notes=[];
let editing=null;
let filter='all';
let pin='';

try{
 notes=JSON.parse(localStorage.getItem('my_diary_notes'))||[];
 pin=localStorage.getItem('my_diary_pin')||'';
 if(localStorage.getItem('my_diary_dark')==='yes')
 document.body.classList.add('dark');
}catch(e){notes=[]}

function saveData(){
 localStorage.setItem('my_diary_notes',JSON.stringify(notes));
}

function today(){
 $('today').textContent=new Date().toLocaleDateString('ar-IQ',{
 weekday:'long',year:'numeric',month:'long',day:'numeric'
 });
}

function render(){
 const list=$('notes');
 list.innerHTML='';
 const query=$('search').value.toLowerCase();

 let data=notes.filter(n=>{
  const matches=(n.title+' '+n.text).toLowerCase().includes(query);
  return matches&&(filter==='all'||n.fav);
 });

 data.sort((a,b)=>b.time-a.time);

 if(!data.length){
  list.innerHTML='<div class="empty"><div style="font-size:55px">📔</div><h3>لا توجد مذكرات بعد</h3><p>ابدأ بكتابة أول ذكرى جميلة!</p></div>';
  return;
 }

 data.forEach(n=>{
  const card=document.createElement('div');
  card.className='note';

  const title=document.createElement('h3');
  title.textContent=n.title||'بدون عنوان';

  const text=document.createElement('p');
  text.textContent=n.text;

  const date=document.createElement('div');
  date.className='date';
  date.textContent=new Date(n.time).toLocaleString('ar-IQ');

  const buttons=document.createElement('div');
  buttons.className='note-buttons';

  const fav=document.createElement('button');
  fav.textContent=n.fav?'❤️':'🤍';
  fav.onclick=()=>toggleFav(n.id);

  const edit=document.createElement('button');
  edit.textContent='✏️ تعديل';
  edit.onclick=()=>openEditor(n.id);

  const del=document.createElement('button');
  del.textContent='🗑️ حذف';
  del.onclick=()=>deleteNote(n.id);

  buttons.append(fav,edit,del);
  card.append(title,text,date,buttons);
  list.append(card);
 });
}

function openEditor(id=null){
 editing=id;
 $('editorTitle').textContent=id?'تعديل المذكرة':'مذكرة جديدة ✍️';
 const n=notes.find(x=>x.id===id);
 $('noteTitle').value=n?n.title:'';
 $('noteText').value=n?n.text:'';
 $('editor').classList.add('show');
}

function closeEditor(){
 $('editor').classList.remove('show');
}

function saveNote(){
 const title=$('noteTitle').value.trim();
 const text=$('noteText').value.trim();

 if(!title&&!text){
  alert('اكتب عنواناً أو محتوى للمذكرة');
  return;
 }

 if(editing!==null){
  const n=notes.find(x=>x.id===editing);
  if(n){
   n.title=title;
   n.text=text;
   n.time=Date.now();
  }
 }else{
  notes.push({
   id:Date.now(),
   title,text,
   time:Date.now(),
   fav:false
  });
 }

 saveData();
 closeEditor();
 render();
}

function deleteNote(id){
 if(!confirm('هل تريد حذف هذه المذكرة؟'))return;
 notes=notes.filter(n=>n.id!==id);
 saveData();
 render();
}

function toggleFav(id){
 const n=notes.find(x=>x.id===id);
 if(n)n.fav=!n.fav;
 saveData();
 render();
}

function toggleDark(){
 document.body.classList.toggle('dark');
 localStorage.setItem(
  'my_diary_dark',
  document.body.classList.contains('dark')?'yes':'no'
 );
}

function exportNotes(){
 const blob=new Blob(
  [JSON.stringify(notes,null,2)],
  {type:'application/json'}
 );
 const url=URL.createObjectURL(blob);
 const a=document.createElement('a');
 a.href=url;
 a.download='my-diary-backup.json';
 a.click();
 URL.revokeObjectURL(url);
}

function setPin(){
 const p=$('pinInput').value.trim();
 if(!/^\d{4,8}$/.test(p)){
  alert('اختر رمزاً من 4 إلى 8 أرقام');
  return;
 }
 pin=p;
 localStorage.setItem('my_diary_pin',pin);
 alert('تم إعداد الرمز');
 closeSettings();
}

function removePin(){
 if(!pin)return alert('لا يوجد رمز محفوظ');
 const old=prompt('أدخل الرمز الحالي');
 if(old!==pin)return alert('الرمز غير صحيح');
 pin='';
 localStorage.removeItem('my_diary_pin');
 alert('تم إلغاء القفل');
 closeSettings();
}

function unlock(){
 if($('unlockPin').value===pin){
  $('lockScreen').style.display='none';
  $('unlockPin').value='';
 }else{
  alert('الرمز غير صحيح');
 }
}

function closeSettings(){
 $('settings').classList.remove('show');
}

today();
render();

if(pin){
 $('lockScreen').style.display='flex';
}

// افتح الإعدادات بضغط مطول على العنوان
document.querySelector('.logo').addEventListener('click',()=>{
 $('settings').classList.add('show');
});
</script>
</body>
</html>
