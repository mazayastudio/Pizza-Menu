# Materi React - Fast React Pizza Co.

Dokumentasi ini menjelaskan konsep-konsep React yang digunakan dalam aplikasi "Fast React Pizza Co." dengan penjelasan baris per baris dari source code.

---

## 1. Rendering Root Component dan Strict Mode

**Lokasi:** `src/index.js` baris 113-118

```javascript
const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(
  <React.StrictMode>
    <App/>
  </React.StrictMode>,
)
```

### Penjelasan:

- **`ReactDOM.createRoot()`** (baris 113): Fungsi yang menciptakan root React untuk rendering aplikasi. Parameter `document.getElementById('root')` mencari elemen HTML dengan id 'root' di file `public/index.html` untuk tempat aplikasi React dimount (dipasang).

- **`root.render()`** (baris 114): Method untuk merender JSX ke dalam DOM. Ini adalah cara modern React 18+ untuk merender aplikasi.

- **`<React.StrictMode>`** (baris 115): Wrapper yang membantu mendeteksi masalah potensial dalam aplikasi selama development. StrictMode:
  - Menjalankan component dua kali untuk mendeteksi side effects yang tidak diinginkan
  - Memberikan warning untuk praktik deprecated
  - Membantu mencegah bugs di production
  - **Hanya aktif saat development**, tidak ada effect di production

- **`<App/>`** (baris 116): Komponen root aplikasi yang berisi struktur utama.

### Pentingnya Proses Ini:

Tanpa rendering ke DOM, React hanya code JavaScript biasa. Proses ini mengubah JSX menjadi HTML yang browser bisa pahami dan tampilkan ke user.

---

## 2. Komponen JSX dan Function Components

**Lokasi:** `src/index.js` baris 50-111

### Apa itu JSX?

JSX adalah syntax extension untuk JavaScript yang memungkinkan kita menulis HTML-like code dalam JavaScript. Browser tidak bisa langsung memahami JSX, jadi React mengubahnya menjadi JavaScript biasa.

### Struktur Komponen Utama:

#### A. App Component (baris 50-58)
```javascript
function App () {
  return (
    <div className="container">
      <Header/>
      <Menu/>
      <Footer/>
    </div>
  )
}
```

**Penjelasan:**
- **Function Component**: `App` adalah function biasa yang mengembalikan JSX
- **Return JSX**: Component harus mengembalikan satu root element (dalam hal ini `<div className="container">`)
- **Nesting Components**: `<Header/>`, `<Menu/>`, dan `<Footer/>` adalah komponen yang di-nest di dalam komponen `App`
- **Composition**: App menggabungkan beberapa komponen kecil menjadi struktur halaman yang lengkap

Ini adalah prinsip dasar React: **membuat komponen kecil yang reusable, kemudian menyusunnya jadi aplikasi yang lebih besar**.

#### B. Header Component (baris 61-69)
```javascript
function Header () {
  return (
    <header className="header">
      <h1>
        Fast React Pizza Co.
      </h1>
    </header>
  )
}
```

**Penjelasan:**
- Component sederhana yang hanya menampilkan header dengan judul
- `className="header"` adalah cara menulis class CSS di JSX (bukan `class` karena `class` adalah reserved keyword di JavaScript)
- Tidak ada state atau props, hanya menampilkan konten statis

#### C. Menu Component (baris 71-83)
```javascript
function Menu () {
  return (
    <main className="menu">
      <h2>Our Menu</h2>
      <Pizza
        name='Pizza Spinaci'
        ingredients='Tomato, mozarella, spinach, and ricotta cheese'
        price={12}
        photoName='pizzas/spinaci.jpg'
      />
    </main>
  )
}
```

**Penjelasan:**
- Component yang menampilkan menu dengan judul "Our Menu"
- Memanggil komponen `<Pizza/>` dengan props (akan dijelaskan di section berikutnya)
- Menggunakan semantic HTML (`<main>` tag untuk konten utama)

#### D. Pizza Component (baris 85-96)
```javascript
function Pizza (props) {
  return (
    <div className='pizza'>
      <img src={props.photoName} alt={props.name}/>
      <div>
        <h3>{props.name}</h3>
        <p>{props.ingredients}</p>
        <span>${props.price}</span>
      </div>
    </div>
  )
}
```

**Penjelasan:**
- **Component dengan Parameter**: `Pizza` menerima parameter `props` yang berisi data
- **Dynamic Content**: Data ditampilkan dengan `{props.nameProperty}` menggunakan curly braces untuk insert JavaScript expression ke dalam JSX
- **Image Tag**: `<img src={props.photoName} alt={props.name}/>` menampilkan gambar pizza
- **Text Interpolation**:
  - `{props.name}` menampilkan nama pizza
  - `{props.ingredients}` menampilkan bahan-bahan
  - `${props.price}` menampilkan harga dengan tanda dollar

#### E. Footer Component (baris 98-111)
```javascript
function Footer () {
  const hour = new Date().getHours()
  const openHour = 12
  const closeHour = 22
  const isOpen = hour >= openHour && hour <= closeHour

  return (
    <footer className="footer">
      <p>{new Date().toLocaleTimeString()}. We're currently open!</p>
      <p>© 2023 Fast React Pizza Co. All rights reserved.</p>
    </footer>
  )
}
```

**Penjelasan:**
- **Logic dalam Component**: Bisa menambahkan JavaScript logic sebelum return statement
- `new Date().getHours()` mendapatkan jam saat ini
- `isOpen` adalah variable yang menentukan apakah toko sedang buka (jam 12 sampai 22)
- `new Date().toLocaleTimeString()` menampilkan waktu saat ini dalam format readable
- Variable `isOpen` tidak digunakan di JSX (bisa dikembangkan untuk conditional rendering)

---

## 3. Passing dan Receiving Props

**Lokasi:** `src/index.js` baris 75-95 (Menu dan Pizza components)

Props adalah cara untuk mengirim data dari parent component ke child component.

### Cara Passing Props:

**Di Menu Component (baris 75-80):**
```javascript
<Pizza
  name='Pizza Spinaci'
  ingredients='Tomato, mozarella, spinach, and ricotta cheese'
  price={12}
  photoName='pizzas/spinaci.jpg'
/>
```

**Penjelasan:**
- `name='Pizza Spinaci'` - String prop ditulis dengan single quotes
- `price={12}` - Number prop ditulis di dalam curly braces untuk JavaScript expression
- `photoName='pizzas/spinaci.jpg'` - String prop untuk path gambar

Setiap prop adalah attribute yang dikirim ke component `Pizza`.

### Cara Receiving Props:

**Di Pizza Component (baris 85):**
```javascript
function Pizza (props) {
  // props adalah object yang berisi semua props yang dikirim
  // Struktur props: { name: 'Pizza Spinaci', ingredients: '...', price: 12, photoName: '...' }
```

**Menggunakan Props di JSX (baris 88-92):**
```javascript
<img src={props.photoName} alt={props.name}/>
<h3>{props.name}</h3>
<p>{props.ingredients}</p>
<span>${props.price}</span>
```

Setiap `{props.propertyName}` mengakses value dari props object.

### Keuntungan Props:

1. **Reusable Component**: Component `Pizza` bisa dipakai berkali-kali dengan data berbeda
2. **Dynamic Content**: Konten berubah berdasarkan props yang dikirim
3. **One-way Data Flow**: Data hanya mengalir dari parent ke child, membuat alur data predictable

### Data yang Belum Digunakan:

```javascript
const pizzaData = [
  // Array of pizza objects dengan struktur yang sama seperti props
]
```

Dalam aplikasi saat ini, `pizzaData` didefinisikan tapi belum digunakan. Di future, data ini bisa di-map untuk menampilkan semua pizza:
```javascript
pizzaData.map(pizza => <Pizza {...pizza} />)
```

---

## 4. Rendering Lists

**Lokasi:** `src/index.js` baris 71-101 (Menu dan Pizza components)

### Apa itu Rendering Lists?

Rendering lists adalah menampilkan banyak items dari array menjadi multiple components. Ini adalah pattern yang sangat umum di React untuk menampilkan data yang dinamis dan dapat berubah.

### Transformasi dari Hardcode ke Dynamic List:

#### Sebelum (Hardcoded Single Item):
```javascript
function Menu () {
  return (
    <main className="menu">
      <h2>Our Menu</h2>
      <Pizza
        name='Pizza Spinaci'
        ingredients='Tomato, mozarella, spinach, and ricotta cheese'
        price={12}
        photoName='pizzas/spinaci.jpg'
      />
    </main>
  )
}
```

#### Sesudah (Dynamic List dengan .map()):
```javascript
function Menu () {
  return (
    <main className="menu">
      <h2>Our Menu</h2>
      <ul className='pizzas'>
        {pizzaData.map((pizza) => (
          <Pizza key={pizza.name} pizzaObj={pizza}/>
        ))}
      </ul>
    </main>
  )
}
```

### Penjelasan Baris per Baris:

#### A. Semantic HTML - `<ul>` dan `<li>` (baris 75, 92)

**Menu Component (baris 75):**
```javascript
<ul className='pizzas'>
  // list items di sini
</ul>
```

**Pizza Component (baris 92):**
```javascript
<li className='pizza'>
  // pizza content di sini
</li>
```

**Penjelasan:**
- **`<ul>`** (Unordered List): Semantic HTML tag untuk daftar items tanpa urutan spesifik
- **`<li>`** (List Item): Semantic HTML tag untuk setiap item di dalam list
- Sebelumnya menggunakan `<div>`, sekarang lebih semantic dan accessible
- Browser dan screen readers bisa lebih baik memahami struktur

#### B. Array.map() - Transform Array ke JSX Elements (baris 76-84)

```javascript
{pizzaData.map((pizza) => (
  <Pizza key={pizza.name} pizzaObj={pizza}/>
))}
```

**Penjelasan:**

1. **`pizzaData.map()`** (baris 76): Method JavaScript untuk transform setiap item di array
   - `pizzaData` adalah array dari 6 pizza objects (lihat baris 5-48)
   - `.map()` adalah higher-order function yang menerima callback function

2. **`(pizza) =>`** (baris 76): Arrow function yang menerima satu parameter
   - `pizza` adalah satu object dari array (misal: `{ name: 'Focaccia', ingredients: '...', price: 6, ... }`)
   - Function ini di-jalankan untuk setiap item di array

3. **`<Pizza key={pizza.name} pizzaObj={pizza}/>`** (baris 83): JSX yang di-return untuk setiap iteration
   - Membuat satu `<Pizza/>` component untuk setiap pizza di array
   - `key={pizza.name}` adalah required prop (akan dijelaskan di bawah)
   - `pizzaObj={pizza}` mengirim seluruh pizza object sebagai props

**Analogi:**
```
Array: [pizza1, pizza2, pizza3, pizza4, pizza5, pizza6]
     ↓ .map()
JSX:  [<Pizza/>, <Pizza/>, <Pizza/>, <Pizza/>, <Pizza/>, <Pizza/>]
```

#### C. Key Attribute - Penting untuk Performance (baris 83)

```javascript
<Pizza key={pizza.name} pizzaObj={pizza}/>
```

**Penjelasan:**

- **`key={pizza.name}`**: Unique identifier untuk setiap item di list
- React menggunakan `key` untuk:
  - **Track which item changed**: Ketika array berubah, React bisa tahu mana yang ditambah, dihapus, atau diubah
  - **Preserve component state**: Jika component memiliki state (akan dipelajari later), key memastikan state berada di item yang benar
  - **Reorder dengan benar**: Jika order berubah, React tahu mana yang mana

**Catatan Penting - Mengapa tidak pakai index?**
```javascript
// ❌ JANGAN: menggunakan index sebagai key
{pizzaData.map((pizza, index) => (
  <Pizza key={index} pizzaObj={pizza}/>
))}

// ✅ BAIK: menggunakan unique identifier
{pizzaData.map((pizza) => (
  <Pizza key={pizza.name} pizzaObj={pizza}/>
))}
```

Menggunakan `index` sebagai key bisa menyebabkan bugs kalau list di-reorder atau ada item yang dihapus.

#### D. Passing Entire Object sebagai Props - Object Spreading

**Opsi 1 - Sekarang (Recommended):**
```javascript
<Pizza key={pizza.name} pizzaObj={pizza}/>

// Di Pizza component:
function Pizza (props) {
  // akses dengan props.pizzaObj.name, props.pizzaObj.price, dll
  <h3>{props.pizzaObj.name}</h3>
}
```

**Opsi 2 - Spread Operator (untuk future):**
```javascript
<Pizza key={pizza.name} {...pizza}/>

// Di Pizza component:
function Pizza ({ name, price, ingredients, photoName, soldOut }) {
  // langsung akses: name, price, ingredients, dll
  <h3>{name}</h3>
}
```

**Penjelasan spread operator:**
- `{...pizza}` mengubah object `{ name: 'Focaccia', price: 6, ... }` menjadi individual props
- Sama dengan menulis: `<Pizza name={pizza.name} price={pizza.price} ingredients={pizza.ingredients} ... />`
- Lebih clean dan singkat, tapi perlu destructuring di child component

#### E. Pizza Component Menerima Props (baris 90-101)

```javascript
function Pizza (props) {
  return (
    <li className='pizza'>
      <img src={props.pizzaObj.photoName} alt={props.pizzaObj.name}/>
      <div>
        <h3>{props.pizzaObj.name}</h3>
        <p>{props.pizzaObj.ingredients}</p>
        <span>${props.pizzaObj.price}</span>
      </div>
    </li>
  )
}
```

**Penjelasan:**

- **`function Pizza (props)`** (baris 90): Menerima satu parameter `props` yang berisi `{ pizzaObj: { ... } }`
- **`props.pizzaObj`** (baris 93, 95, 96, 97): Mengakses pizza object dari props
- **`{props.pizzaObj.name}`** (baris 95): Menampilkan nama pizza
- **`{props.pizzaObj.ingredients}`** (baris 96): Menampilkan bahan-bahan
- **`${props.pizzaObj.price}`** (baris 97): Menampilkan harga dengan string interpolation

### Complete Data Flow:

```
pizzaData (Array)
    ↓
Menu component
    ↓
.map() - transform array menjadi JSX
    ↓
<ul><li><Pizza/></li>...</ul>
    ↓
Pizza component menerima props.pizzaObj
    ↓
Display gambar, nama, bahan, harga
```

### Keuntungan Rendering Lists Dengan .map():

1. **Dynamic**: Bisa menampilkan banyak items tanpa repetisi code
2. **Maintainable**: Kalau data berubah, component otomatis update
3. **Scalable**: Bisa handle 10 items atau 1000 items dengan code yang sama
4. **Reusable**: Pizza component dipakai untuk setiap item

### Contoh Modifikasi - Conditional Rendering dengan List:

```javascript
// Highlight sold-out items
function Pizza ({ pizzaObj }) {
  return (
    <li className={pizzaObj.soldOut ? 'pizza sold-out' : 'pizza'}>
      // ... rest of component
    </li>
  )
}
```

Menggunakan ternary operator untuk conditional CSS class berdasarkan `soldOut` property.

### Troubleshooting Common Errors:

**Error 1: "Each child in a list should have a unique 'key' prop"**
- Solusi: Tambahkan `key` attribute ke setiap item yang di-render dari list

**Error 2: Component tidak update ketika array berubah**
- Possible cause: Array reference tidak berubah (technical issue dengan state)
- Solusi: Pastikan menggunakan `.map()` di component yang benar

**Error 3: Items melompat/berubah urutan**
- Possible cause: Menggunakan index sebagai key
- Solusi: Gunakan unique identifier seperti `pizza.name` atau `pizza.id`

---

## 5. Macam-macam Cara Menambahkan Styling di React

**Lokasi:** `src/index.css` dan penggunaan di components

### A. CSS Classes (Cara Paling Umum)

**File: `src/index.css`**

#### 1. Global Styling (baris 1-22)

```css
@import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:ital,wght@0,300;0,400;0,500;1,300&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  font-size: 62.5%;  /* Mengubah 1rem = 10px untuk calculation yang lebih mudah */
}

body {
  font-family: 'Roboto Mono', sans-serif;
  color: #252525;
  background-color: #f7f2e9;
  border-bottom: 1.6rem solid #edc84b;
  padding: 3.2rem;
}
```

**Penjelasan:**
- **Universal Selector** (`*`): Reset default margin dan padding dari semua elemen
- **Box-sizing: border-box**: Membuat width dan height mencakup padding dan border
- **Font Import**: Menggunakan Google Fonts (Roboto Mono)
- **`font-size: 62.5%` di html**: Trick untuk membuat 1rem = 10px (16px × 62.5% = 10px), memudahkan calculation

#### 2. Container Layout (baris 24-32)

```css
.container {
  max-width: 80rem;
  margin: 0 auto;  /* Center horizontally */

  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4.8rem;
}
```

**Digunakan di:** `<div className="container">` di App component

**Penjelasan:**
- **Flexbox Layout**: Mengatur child elements dalam column (vertical)
- `align-items: center`: Centered horizontally
- `gap: 4.8rem`: Jarak antar child elements
- `max-width: 80rem`: Pembatas lebar untuk responsive design

#### 3. Header Styling (baris 36-72)

```css
.header h1 {
  color: #edc84b;
  text-transform: uppercase;
  text-align: center;
  font-size: 5.2rem;
  letter-spacing: 3px;
  position: relative;
}

.header h1::before,
.header h1::after {
  display: block;
  content: '';
  height: 3px;
  width: 4rem;
  background-color: #edc84b;
  position: absolute;
  top: calc(50% - 1px);
}
```

**Penjelasan:**
- **Pseudo-elements** (`::before`, `::after`): Membuat dekorasi garis di kiri dan kanan h1
- `content: ''`: Harus ada untuk pseudo-element muncul
- `position: absolute`: Positioning dekorasi relative to `position: relative` di parent
- `calc(50% - 1px)`: Dynamic calculation untuk centering vertically

#### 4. Menu dan Pizza Styling (baris 76-151)

```css
.pizzas {
  display: grid;
  grid-template-columns: 1fr 1fr;  /* 2 kolom equal width */
  gap: 4.8rem;
}

.pizza {
  display: flex;
  gap: 3.2rem;
}

.pizza img {
  width: 12rem;
  aspect-ratio: 1;  /* Square image */
  align-self: start;
}

.pizza.sold-out {
  color: #888;
}

.pizza.sold-out img {
  filter: grayscale();  /* Grayscale filter untuk sold-out items */
  opacity: 0.8;
}
```

**Penjelasan:**
- **Grid Layout**: 2 kolom untuk display pizzas
- **Flexbox di .pizza**: Arrange image dan description horizontally
- **aspect-ratio: 1**: Membuat image square (1:1)
- **CSS Classes untuk State**: `.pizza.sold-out` untuk styling item yang terjual habis
- **CSS Filter**: `grayscale()` dan `opacity` untuk visual feedback

#### 5. Spacing System (baris 181-187)

```css
/*
SPACING SYSTEM (px)
2 / 4 / 8 / 12 / 16 / 24 / 32 / 40 / 48 / 64 / 80 / 96 / 128

FONT SIZE SYSTEM (px)
10 / 12 / 14 / 16 / 18 / 20 / 24 / 30 / 36 / 44 / 52 / 62 / 74 / 86 / 98
*/
```

**Penjelasan:**
- **Design System**: Menggunakan spacing dan font size yang consistent
- Memudahkan maintainability dan consistency visual
- Semua spacing dan font size di project mengikuti system ini

### B. Menggunakan CSS Classes di React

**Di Components:**

```javascript
// Header component
<header className="header">

// Menu component
<main className="menu">

// Pizza component
<div className='pizza'>

// Conditional class (untuk future development)
// <div className={isOpen ? 'pizza' : 'pizza sold-out'}>
```

**Penting:** Di React gunakan `className` bukan `class` (karena `class` adalah reserved keyword di JavaScript)

### C. CSS Reset dan Best Practices

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
```

**Penjelasan:**
- Menghilangkan default margin/padding dari browser
- Membuat styling lebih predictable dan konsisten
- `box-sizing: border-box`: Ukuran element mencakup border dan padding (lebih intuitif)

---

## 6. Array Data dan Map (Future Development)

**Lokasi:** `src/index.js` baris 5-48

```javascript
const pizzaData = [
  {
    name: 'Focaccia',
    ingredients: 'Bread with italian olive oil and rosemary',
    price: 6,
    photoName: 'pizzas/focaccia.jpg',
    soldOut: false,
  },
  // ... lebih banyak pizza objects
]
```

### Struktur Data:

- Array berisi objects
- Setiap object memiliki properties: `name`, `ingredients`, `price`, `photoName`, `soldOut`
- Structure ini perfect untuk di-map ke Pizza components

### Cara Menggunakan (Future):

```javascript
// Dalam Menu component
function Menu () {
  return (
    <main className="menu">
      <h2>Our Menu</h2>
      {pizzaData.map(pizza => (
        <Pizza key={pizza.name} {...pizza} />
      ))}
    </main>
  )
}
```

**Penjelasan:**
- `.map()` mengubah array menjadi array dari JSX elements
- `key={pizza.name}` membantu React mengidentifikasi mana yang berubah (untuk performance)
- `{...pizza}` adalah spread operator, mengubah object properties menjadi props

---

## Ringkasan Konsep React yang Dipelajari

1. ✅ **React Setup & Rendering**: Cara merender React app ke DOM dengan `ReactDOM.createRoot()`
2. ✅ **JSX Syntax**: Menulis HTML-like code dalam JavaScript
3. ✅ **Function Components**: Membuat components sebagai functions yang return JSX
4. ✅ **Component Composition**: Membuat struktur aplikasi dari komponen-komponen kecil
5. ✅ **Props**: Mengirim data dari parent ke child components
6. ✅ **Dynamic Content**: Menggunakan JavaScript expressions dalam JSX dengan curly braces
7. ✅ **Array.map()**: Transform array menjadi multiple React components
8. ✅ **Key Attribute**: Unique identifier untuk list items (penting untuk React performance)
9. ✅ **Semantic HTML**: Menggunakan `<ul>`, `<li>` untuk list items
10. ✅ **CSS Classes**: Styling components dengan external CSS file
11. ✅ **Flexbox & Grid**: Layout dengan modern CSS
12. ✅ **React.StrictMode**: Development tool untuk mendeteksi bugs

---

## Tips untuk Pengembangan Lebih Lanjut

### 1. ✅ Rendering Multiple Pizzas (SUDAH IMPLEMENTED)
Sekarang menggunakan `pizzaData.map()` untuk render semua 6 pizzas dari array secara dinamis dengan semantic `<ul>` dan `<li>` tags.

**Next Step:** Tambahkan filter atau search untuk filter pizzas berdasarkan ingredient atau price range.

### 2. Conditional Rendering - Show/Hide based on Condition
Gunakan ternary operator untuk show/hide konten:

```javascript
// Di Footer - Show opening hours
{isOpen ? <p>We're open!</p> : <p>Sorry, we're closed</p>}

// Di Pizza - Show sold-out status
{pizza.soldOut ? <p className="sold-out">SOLD OUT</p> : <p>Available</p>}
```

### 3. ✅ Conditional CSS Classes (READY TO IMPLEMENT)
Tambahkan dynamic class berdasarkan `soldOut` property:

```javascript
function Pizza ({ pizzaObj }) {
  return (
    <li className={pizzaObj.soldOut ? 'pizza sold-out' : 'pizza'}>
      {/* content */}
    </li>
  )
}
```

CSS already ada di `index.css`:
```css
.pizza.sold-out {
  color: #888;
}
.pizza.sold-out img {
  filter: grayscale();
  opacity: 0.8;
}
```

### 4. Destructuring Props - Cleaner Code
Alih-alih `props.pizzaObj.name`, destructure di function parameter:

```javascript
// Sebelum:
function Pizza (props) {
  return <h3>{props.pizzaObj.name}</h3>
}

// Sesudah (lebih clean):
function Pizza ({ pizzaObj }) {
  return <h3>{pizzaObj.name}</h3>
}

// Atau destructure lebih dalam:
function Pizza ({ pizzaObj: { name, price, ingredients, photoName } }) {
  return <h3>{name}</h3>
}
```

### 5. State Management dengan useState (Untuk Future)
Ketika perlu data yang berubah (interaktif), gunakan `useState` hook:

```javascript
import { useState } from 'react'

function Menu () {
  const [cart, setCart] = useState([])

  return (
    // component yang bisa add/remove items dari cart
  )
}
```

### 6. Spread Operator untuk Props
Gunakan spread operator untuk pass multiple props lebih singkat:

```javascript
// Opsi 1 - Sekarang:
<Pizza key={pizza.name} pizzaObj={pizza}/>

// Opsi 2 - Spread (perlu destructure di Pizza):
<Pizza key={pizza.name} {...pizza}/>

// Pizza component:
function Pizza ({ name, price, ingredients, photoName, soldOut }) {
  // bisa langsung gunakan name, price, dll
}
```

---

**Last Updated:** 2026-04-19
**Author Learning Resources:** React Official Docs, Create React App Documentation
