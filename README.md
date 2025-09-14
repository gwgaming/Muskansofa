<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Muskan Sofa Repair - Admin Editable</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  body {font-family: system-ui, sans-serif;}
  .hero-bg{background-size:cover;background-position:center;}
  .gallery-item img{transition:transform .3s}
  .gallery-item img:hover{transform:scale(1.05)}
</style>
</head>
<body class="bg-gray-50 text-gray-800">

<!-- === ADMIN BUTTON === -->
<button id="openAdmin"
  class="fixed top-4 right-4 bg-orange-500 text-white px-4 py-2 rounded shadow-lg z-50">
  Admin
</button>

<!-- === HERO === -->
<header class="hero-bg h-96 flex flex-col justify-center items-center text-center text-white bg-gray-800" id="hero">
  <h1 class="text-4xl md:text-5xl font-extrabold">Muskan Sofa Repair</h1>
  <p class="mt-2 text-lg">Reviving Your Comfort, One Sofa at a Time.</p>
</header>

<!-- === SERVICES === -->
<section id="services" class="py-12 px-6 bg-gray-100">
  <div class="max-w-6xl mx-auto grid md:grid-cols-3 gap-6">
    <div class="bg-white rounded shadow overflow-hidden service" data-index="0">
      <img class="w-full h-56 object-cover service-img">
      <h3 class="p-4 text-xl font-bold">Upholstery & Fabric Change</h3>
    </div>
    <div class="bg-white rounded shadow overflow-hidden service" data-index="1">
      <img class="w-full h-56 object-cover service-img">
      <h3 class="p-4 text-xl font-bold">Frame & Leg Repair</h3>
    </div>
    <div class="bg-white rounded shadow overflow-hidden service" data-index="2">
      <img class="w-full h-56 object-cover service-img">
      <h3 class="p-4 text-xl font-bold">Cushion Refill & Replacement</h3>
    </div>
  </div>
</section>

<!-- === GALLERY === -->
<section id="gallery" class="py-12 px-6">
  <h2 class="text-3xl font-bold text-center mb-6">Project Gallery</h2>
  <div id="galleryGrid" class="grid sm:grid-cols-2 md:grid-cols-3 gap-6 max-w-6xl mx-auto"></div>
</section>

<!-- === CONTACT === -->
<section class="bg-gray-900 text-white py-12 text-center">
  <p class="text-lg">Call/WhatsApp: <span id="phoneTxt"></span></p>
</section>

<footer class="bg-gray-800 text-gray-300 py-4 text-center">© 2025 Muskan Sofa Repair</footer>

<!-- === ADMIN PANEL === -->
<div id="adminPanel"
     class="fixed inset-0 bg-black bg-opacity-60 hidden items-center justify-center z-50">
  <div class="bg-white rounded p-6 w-96 space-y-4 relative">
    <button id="closeAdmin" class="absolute top-2 right-3 text-2xl">&times;</button>
    <h2 class="text-xl font-bold mb-4">Admin Panel</h2>

    <label class="block">Phone:
      <input id="phoneInput" class="w-full border p-2 mt-1 rounded">
    </label>

    <label class="block">Hero Image:
      <input id="heroInput" type="file" accept="image/*" class="mt-1">
    </label>

    <label class="block">Service Images (3):
      <input id="serviceInput" type="file" accept="image/*" multiple class="mt-1">
    </label>

    <label class="block">Gallery Images (multiple):
      <input id="galleryInput" type="file" accept="image/*" multiple class="mt-1">
    </label>

    <button id="saveBtn"
      class="bg-orange-500 text-white w-full py-2 rounded mt-4">Save Changes</button>
  </div>
</div>

<script>
/* --------- Default Data --------- */
const defaultData = {
  phone: "9015178506",
  hero: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?auto=format&fit=crop&w=1200&q=80",
  services: [
    "https://images.unsplash.com/photo-1598300056483-940b0d9a86aa?auto=format&fit=crop&w=800&q=80",
    "https://images.unsplash.com/photo-1625075772049-9e4429e6b7a3?auto=format&fit=crop&w=800&q=80",
    "https://images.unsplash.com/photo-1600585154394-19d51b27d9d0?auto=format&fit=crop&w=800&q=80"
  ],
  gallery: [
    "https://images.unsplash.com/photo-1598300056483-940b0d9a86aa?auto=format&fit=crop&w=800&q=80",
    "https://images.unsplash.com/photo-1625075772049-9e4429e6b7a3?auto=format&fit=crop&w=800&q=80",
    "https://images.unsplash.com/photo-1600585154394-19d51b27d9d0?auto=format&fit=crop&w=800&q=80"
  ]
};

function loadData() {
  return JSON.parse(localStorage.getItem("muskanSite") || JSON.stringify(defaultData));
}
function saveData(d) {
  localStorage.setItem("muskanSite", JSON.stringify(d));
}
function render() {
  const d = loadData();
  document.getElementById("phoneTxt").textContent = d.phone;
  document.getElementById("hero").style.backgroundImage =
    `linear-gradient(rgba(0,0,0,.5),rgba(0,0,0,.5)),url('${d.hero}')`;

  document.querySelectorAll(".service-img").forEach((img,i)=>{
    img.src = d.services[i];
  });

  const grid = document.getElementById("galleryGrid");
  grid.innerHTML = "";
  d.gallery.forEach(src=>{
    const div = document.createElement("div");
    div.className="gallery-item overflow-hidden rounded shadow";
    div.innerHTML=`<img src="${src}" class="w-full h-64 object-cover">`;
    grid.appendChild(div);
  });
}
render();

/* --------- Admin Logic --------- */
const adminPanel = document.getElementById("adminPanel");
document.getElementById("openAdmin").onclick = ()=>{
  const d = loadData();
  document.getElementById("phoneInput").value = d.phone;
  adminPanel.classList.remove("hidden");
};
document.getElementById("closeAdmin").onclick = ()=> adminPanel.classList.add("hidden");

document.getElementById("saveBtn").onclick = ()=>{
  const d = loadData();
  d.phone = document.getElementById("phoneInput").value.trim();

  const heroFile = document.getElementById("heroInput").files[0];
  const serviceFiles = document.getElementById("serviceInput").files;
  const galleryFiles = document.getElementById("galleryInput").files;

  const promises = [];
  if(heroFile) promises.push(readAsDataURL(heroFile).then(u=>d.hero=u));
  if(serviceFiles.length){
    const arr = Array.from(serviceFiles).map(f=>readAsDataURL(f));
    promises.push(Promise.all(arr).then(u=>d.services = u));
  }
  if(galleryFiles.length){
    const arr = Array.from(galleryFiles).map(f=>readAsDataURL(f));
    promises.push(Promise.all(arr).then(u=>d.gallery = u));
  }

  Promise.all(promises).then(()=>{
    saveData(d);
    render();
    adminPanel.classList.add("hidden");
  });
};

function readAsDataURL(file){
  return new Promise(res=>{
    const r = new FileReader();
    r.onload = e=>res(e.target.result);
    r.readAsDataURL(file);
  });
}
</script>
</body>
</html>
