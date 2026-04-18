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

## 4. Macam-macam Cara Menambahkan Styling di React

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

## 5. Array Data dan Map (Future Development)

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
7. ✅ **CSS Classes**: Styling components dengan external CSS file
8. ✅ **Flexbox & Grid**: Layout dengan modern CSS
9. ✅ **React.StrictMode**: Development tool untuk mendeteksi bugs

---

## Tips untuk Pengembangan Lebih Lanjut

### 1. Rendering Multiple Pizzas
Gunakan `pizzaData.map()` untuk render semua pizza dari array alih-alih hardcode satu pizza.

### 2. Conditional Rendering
Gunakan variable `isOpen` di Footer untuk show/hide konten berdasarkan jam buka toko:
```javascript
{isOpen ? <p>We're open!</p> : <p>Sorry, we're closed</p>}
```

### 3. Conditional CSS Classes
Tambahkan class dinamis berdasarkan state:
```javascript
<div className={pizza.soldOut ? 'pizza sold-out' : 'pizza'}>
```

### 4. State Management (Untuk Future)
Ketika perlu data yang berubah, gunakan `useState` hook:
```javascript
const [count, setCount] = useState(0)
```

### 5. Destructuring Props
Alih-alih `props.name`, gunakan destructuring untuk cleaner code:
```javascript
function Pizza ({ name, ingredients, price, photoName }) {
  // langsung gunakan name, ingredients, dll
}
```

---

**Last Updated:** 2026-04-19
**Author Learning Resources:** React Official Docs, Create React App Documentation
