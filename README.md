<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Origami Book Finder</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  html { scroll-behavior: smooth; }
  .book-card:hover { transform: scale(1.03); transition: transform 0.2s; }
</style>
</head>
<body class="bg-slate-50 text-slate-900 dark:bg-slate-900 dark:text-slate-100">

<!-- Navbar -->
<header class="fixed top-0 w-full bg-white dark:bg-slate-800 shadow z-50">
  <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 flex justify-between h-16 items-center">
    <div class="font-bold text-xl">Origami Book Finder</div>
    <nav class="space-x-4 hidden md:flex">
      <a href="#home" class="hover:underline">Home</a>
      <a href="#categories" class="hover:underline">Categories</a>
      <a href="#tips" class="hover:underline">Tips</a>
      <a href="#where" class="hover:underline">Where to Find</a>
      <a href="#community" class="hover:underline">Community Picks</a>
      <a href="#about" class="hover:underline">About</a>
      <a href="#submit" class="hover:underline">Submit Review</a>
    </nav>
    <button id="darkToggle" class="px-3 py-1 bg-indigo-600 text-white rounded">Dark/Light</button>
  </div>
</header>

<main class="pt-20 max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 space-y-20">

<!-- Home -->
<section id="home" class="text-center py-12">
  <h1 class="text-4xl font-extrabold mb-4">Find the Perfect Origami Book</h1>
  <p class="text-slate-600 dark:text-slate-300 mb-6">Step-by-step instructions for all levels. Browse, filter, and save your favorites!</p>
  <a href="#categories" class="px-6 py-3 bg-indigo-600 text-white rounded">Browse Categories</a>
</section>

<!-- Categories -->
<section id="categories">
  <h2 class="text-3xl font-bold mb-6">Categories</h2>
  
  <!-- Filters -->
  <div class="flex flex-col sm:flex-row gap-3 mb-6">
    <select id="levelFilter" class="px-3 py-2 border rounded">
      <option value="All">All Levels</option>
      <option value="Beginner">Beginner</option>
      <option value="Kids">Kids</option>
      <option value="Intermediate">Intermediate</option>
      <option value="Advanced">Advanced</option>
    </select>
    <input type="text" id="searchInput" placeholder="Search books..." class="px-3 py-2 border rounded flex-1">
  </div>

  <!-- Books Grid -->
  <div id="booksGrid" class="grid gap-6 grid-cols-1 sm:grid-cols-2 lg:grid-cols-3"></div>

  <!-- Wishlist -->
  <div class="mt-10">
    <h3 class="text-xl font-semibold mb-2">Your Wishlist</h3>
    <ul id="wishlist" class="list-disc ml-5 text-slate-600 dark:text-slate-400"></ul>
  </div>
</section>

<!-- Tips -->
<section id="tips">
  <h2 class="text-3xl font-bold mb-6">Tips for Choosing the Right Origami Book</h2>
  <div class="grid sm:grid-cols-2 gap-4">
    <div class="p-4 bg-white dark:bg-slate-800 rounded shadow">Match your skill level. Start with books labeled Beginner, Intermediate, Advanced.</div>
    <div class="p-4 bg-white dark:bg-slate-800 rounded shadow">Check diagrams. Clear diagrams are easier than photos for learning folds.</div>
    <div class="p-4 bg-white dark:bg-slate-800 rounded shadow">Look for variety. Books with many models keep learning fun.</div>
    <div class="p-4 bg-white dark:bg-slate-800 rounded shadow">Paper included? Some beginner kits come with paper, useful for kids or gifts.</div>
  </div>
</section>

<!-- Where to Find -->
<section id="where">
  <h2 class="text-3xl font-bold mb-6">Where to Find Origami Books</h2>
  <ul class="grid sm:grid-cols-2 gap-4">
    <li class="p-4 bg-white dark:bg-slate-800 rounded shadow">Online retailers: Amazon, Book Depository, Barnes & Noble.</li>
    <li class="p-4 bg-white dark:bg-slate-800 rounded shadow">Specialty publishers: Tuttle Publishing and other origami publishers.</li>
    <li class="p-4 bg-white dark:bg-slate-800 rounded shadow">Libraries & used bookstores for older or rare titles.</li>
    <li class="p-4 bg-white dark:bg-slate-800 rounded shadow">Origami conventions & clubs often sell rare books.</li>
  </ul>
</section>

<!-- Community Picks -->
<section id="community">
  <h2 class="text-3xl font-bold mb-6">Community Picks</h2>
  <ul class="grid sm:grid-cols-3 gap-4" id="communityPicks"></ul>
</section>

<!-- About -->
<section id="about">
  <h2 class="text-3xl font-bold mb-4">About</h2>
  <p class="text-slate-600 dark:text-slate-300">Origami Book Finder helps folders of every level discover instruction books tailored to their interests and skills. Curated titles, wishlist functionality, and community recommendations all in one place.</p>
</section>

<!-- Submit Review -->
<section id="submit" class="mt-10">
  <h2 class="text-3xl font-bold mb-4">Submit a Review</h2>
  <form id="reviewForm" class="grid gap-3 max-w-md">
    <input type="text" placeholder="Your Name" required class="px-3 py-2 border rounded">
    <input type="text" placeholder="Book Title" required class="px-3 py-2 border rounded">
    <textarea placeholder="Your Review" rows="4" required class="px-3 py-2 border rounded"></textarea>
    <button type="submit" class="px-4 py-2 bg-indigo-600 text-white rounded">Submit</button>
  </form>
  <div id="reviewThanks" class="mt-4 hidden p-4 bg-green-100 dark:bg-green-800 rounded">Thank you for your review!</div>
</section>

</main>

<script>
// Dark mode toggle
document.getElementById('darkToggle').onclick = () => document.body.classList.toggle('dark');

// Book data
const books = [
  {title:"Solid Origami",author:"Shuzo Fujimoto",level:"Advanced",theme:"Geometric",description:"Geometric models, 3D solids, stars, and modular origami.",cover:"covers/solid-origami.jpg"},
  {title:"Perfectly Mindful Origami",author:"Mark Bolitho",level:"Intermediate",theme:"Mindful",description:"The Art and Craft of Geometric Origami.",cover:"covers/perfectly-mindful-origami.jpg"},
  {title:"New Generation Of Origami",author:"Makoto Yamaguchi",level:"Intermediate",theme:"Modern",description:"Modern origami designs.",cover:"covers/new-generation-of-origami.jpg"},
  {title:"The Usborne Book of Origami",author:"Kate Needham",level:"Beginner",theme:"General",description:"Step-by-step directions for creating a variety of origami projects.",cover:"covers/usborne-origami.jpg"},
  {title:"Origami for Everyone",author:"Didier Boursin",level:"Intermediate",theme:"Collection",description:"68 masterpieces.",cover:"covers/origami-for-everyone.jpg"},
  {title:"The Best of Origami",author:"Various",level:"Advanced",theme:"Collection",description:"New models by contemporary folders.",cover:"covers/best-of-origami.jpg"},
  {title:"Origami for Beginners",author:"Florence Temko",level:"Beginner",theme:"General",description:"Gentle introduction with essential folds and simple models.",cover:"covers/origami-for-beginners.jpg"},
  {title:"Easy Origami",author:"John Montroll",level:"Beginner",theme:"General",description:"Large collection of easy projects.",cover:"covers/easy-origami.jpg"},
  {title:"Absolute Beginner's Origami",author:"Nick Robinson",level:"Beginner",theme:"General",description:"Step-by-step lessons focused on basic folds.",cover:"covers/absolute-beginners-origami.jpg"},
  {title:"Origami for Children",author:"Mari Ono",level:"Kids",theme:"Kids",description:"Bright, easy-to-follow projects.",cover:"covers/origami-for-children.jpg"},
  {title:"My First Origami Kit",author:"Tuttle Publishing",level:"Kids",theme:"Kids",description:"Beginner kit with colorful papers.",cover:"covers/my-first-origami-kit.jpg"},
  {title:"Origami Zoo",author:"Joel Stern",level:"Kids",theme:"Animals",description:"Animal-themed models for younger folders.",cover:"covers/origami-zoo.jpg"},
  {title:"Origami Omnibus",author:"Kunihiko Kasahara",level:"Intermediate",theme:"Collection",description:"Intermediate models — animals, flowers, classics.",cover:"covers/origami-omnibus.jpg"},
  {title:"Amazing Origami",author:"Kunihiko Kasahara",level:"Intermediate",theme:"General",description:"Wide range of intermediate projects.",cover:"covers/amazing-origami.jpg"},
  {title:"Origami Design Secrets",author:"Robert J. Lang",level:"Advanced",theme:"Theory",description:"Deep dive into origami design and mathematical techniques.",cover:"covers/origami-design-secrets.jpg"},
  {title:"Works of Satoshi Kamiya",author:"Satoshi Kamiya",level:"Advanced",theme:"Masterpieces",description:"Detailed models for experienced folders.",cover:"covers/works-of-satoshi-kamiya.jpg"},
  {title:"Origami Insects and Their Kin",author:"Robert J. Lang",level:"Advanced",theme:"Animals",description:"Complex insect models and advanced insect-inspired designs.",cover:"covers/origami-insects.jpg"},
  {title:"Origami Flowers",author:"Mari Ono",level:"Beginner",theme:"Flowers",description:"Beautiful flower projects.",cover:"covers/origami-flowers.jpg"}
];

// Display books
const booksGrid = document.getElementById('booksGrid');
const wishlistEl = document.getElementById('wishlist');
let wishlist = JSON.parse(localStorage.getItem('wishlist')||'[]');

function renderWishlist() {
  wishlistEl.innerHTML = wishlist.map(b => `<li>${b.title} — ${b.author}</li>`).join('');
}

function renderBooks() {
  const query = document.getElementById('searchInput').value.toLowerCase();
  const level = document.getElementById('levelFilter').value;
  booksGrid.innerHTML = '';
  books.filter(b => (level==='All'||b.level===level) && (b.title.toLowerCase().includes(query)||b.author.toLowerCase().includes(query)||b.description.toLowerCase().includes(query)))
    .forEach(b => {
      const div = document.createElement('div');
      div.className='book-card bg-white dark:bg-slate-800 p-4 rounded shadow cursor-pointer';
      div.innerHTML = `
        <div class="flex gap-3">
          <div class="w-24 h-32 overflow-hidden rounded"><img src="${b.cover}" alt="${b.title}" class="w-full h-full object-cover"></div>
          <div class="flex-1">
            <h3 class="font-bold text-lg">${b.title}</h3>
            <p class="text-sm">${b.author} — ${b.level}</p>
            <p class="text-sm mt-1">${b.description}</p>
            <button class="mt-2 px-3 py-1 bg-indigo-600 text-white rounded add-wishlist">Add to Wishlist</button>
          </div>
        </div>`;
      booksGrid.appendChild(div);
      div.querySelector('.add-wishlist').onclick = () => {
        if(!wishlist.some(x=>x.title===b.title)) wishlist.push(b);
        localStorage.setItem('wishlist',JSON.stringify(wishlist));
        renderWishlist();
      }
    });
}

document.getElementById('searchInput').oninput = renderBooks;
document.getElementById('levelFilter').onchange = renderBooks;

renderWishlist();
renderBooks();

// Community Picks (simple: first 6 books)
const communityEl = document.getElementById('communityPicks');
books.slice(0,6).forEach(b=>{
  const li = document.createElement('li');
  li.className='p-4 bg-white dark:bg-slate-800 rounded shadow';
  li.innerHTML = `<strong>${b.title}</strong><br>${b.author} — ${b.level}`;
  communityEl.appendChild(li);
});

// Submit Review
const reviewForm = document.getElementById('reviewForm');
const reviewThanks = document.getElementById('reviewThanks');
reviewForm.onsubmit = e=>{
  e.preventDefault();
  reviewForm.reset();
  reviewThanks.classList.remove('hidden');
  setTimeout(()=>reviewThanks.classList.add('hidden'),3000);
};
</script>

</body>
</html>
