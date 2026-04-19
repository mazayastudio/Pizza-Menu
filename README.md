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

## 5. Conditional Rendering dengan && (Logical AND Operator)

**Lokasi:** `src/index.js` baris 78-90 (Menu), baris 117-126 (Footer)

### Apa itu Conditional Rendering?

Conditional rendering adalah menampilkan atau menyembunyikan JSX elements berdasarkan kondisi tertentu. Ini memungkinkan UI berubah dinamis sesuai state atau data.

### Tiga Cara Conditional Rendering di React:

#### 1. **Ternary Operator** (condition ? true : false)
```javascript
{isOpen ? <p>We're open!</p> : <p>We're closed!</p>}
```
- Menampilkan dua konten berbeda berdasarkan kondisi
- Selalu render salah satu dari dua opsi

#### 2. **Logical AND (&&)** - Yang akan kita pelajari
```javascript
{numPizzas > 0 && <ul className="pizzas">...</ul>}
```
- Menampilkan konten hanya jika kondisi TRUE
- Tidak menampilkan apa-apa jika kondisi FALSE
- Lebih clean dan readable untuk true-only case

#### 3. **If Statement** (di JavaScript logic)
```javascript
let content;
if (isOpen) {
  content = <div className="order">...</div>;
}
return <footer>{content}</footer>;
```
- Menggunakan JavaScript logic sebelum return

### Pattern: Logical AND (&&) - Fokus Pembahasan

#### A. Menu Component - Render List hanya jika Ada Pizzas (baris 71-93)

```javascript
function Menu () {
  const pizzas = pizzaData;
  const numPizzas = pizzas.length;

  return (
    <main className="menu">
      <h2>Our Menu</h2>
      {numPizzas > 0 && (
        <ul className="pizzas">
          {pizzas.map((pizza) => (
            <Pizza key={pizza.name} pizzaObj={pizza} />
          ))}
        </ul>
      )}
    </main>
  );
}
```

**Penjelasan Baris per Baris:**

1. **`const pizzas = pizzaData;`** (baris 72)
   - Membuat variable `pizzas` yang refer ke array `pizzaData`
   - Ini adalah shorthand/alias untuk `pizzaData`

2. **`const numPizzas = pizzas.length;`** (baris 73)
   - Menghitung jumlah items di array
   - `pizzaData` memiliki 6 items, jadi `numPizzas = 6`

3. **`{numPizzas > 0 && (...)`** (baris 78)
   - **Kondisi**: `numPizzas > 0` (apakah ada minimum 1 pizza?)
   - **Operator &&**: Logical AND
   - Jika TRUE: render `<ul>...</ul>`
   - Jika FALSE: render `null` (tidak ada apa-apa)

**Bagaimana && Bekerja di JavaScript:**

```javascript
// Pattern: condition && expression

true && "Hello"   // hasil: "Hello" (kanan di-evaluate)
false && "Hello"  // hasil: false (kanan tidak di-evaluate)

5 > 0 && "Five is greater"  // hasil: "Five is greater"
5 < 0 && "Five is greater"  // hasil: false (tidak ditampilkan)
```

**Di React JSX:**

```javascript
// Jika numPizzas = 6 (true)
{6 > 0 && <ul>...</ul>}
// render: <ul>...</ul>

// Jika numPizzas = 0 (false)
{0 > 0 && <ul>...</ul>}
// render: null (tidak ada apa-apa)
```

**Use Case:**
- Tampilkan menu list hanya jika ada pizzas
- Jika tidak ada pizzas (misal belum load dari API), tidak tampil list kosong yang aneh

#### B. Footer Component - Render Order Section hanya jika Open (baris 108-129)

```javascript
function Footer () {
  const hour = new Date().getHours()
  const openHour = 8;
  const closeHour = 22
  const isOpen = hour >= openHour && hour <= closeHour

  return (
    <footer className="footer">
      {isOpen && (
        <div className="order">
          <p>
            {new Date().toLocaleTimeString()}. We're currently open until{" "}
            {closeHour}:00. Come visit us or order online.
          </p>
          <button className="btn">Order Now</button>
          <p>© 2023 Fast React Pizza Co. All rights reserved.</p>
        </div>
      )}
    </footer>
  );
}
```

**Penjelasan Baris per Baris:**

1. **`const hour = new Date().getHours()`** (baris 109)
   - Mendapatkan jam saat ini (0-23)
   - Contoh: pukul 14:30 → `hour = 14`

2. **`const openHour = 8; const closeHour = 22`** (baris 110-111)
   - Jam buka: 08:00 (8 pagi)
   - Jam tutup: 22:00 (10 malam)

3. **`const isOpen = hour >= openHour && hour <= closeHour`** (baris 112)
   - **Kondisi compound**: Dua kondisi dengan &&
   - `hour >= 8` AND `hour <= 22`
   - Contoh: jam 14:00 → `14 >= 8 && 14 <= 22` → `true && true` → `true`
   - Contoh: jam 23:00 → `23 >= 8 && 23 <= 22` → `true && false` → `false`

4. **`{isOpen && (...)`** (baris 117)
   - Jika `isOpen` TRUE: render order section dengan pesan dan button
   - Jika `isOpen` FALSE: render nothing

5. **`{new Date().toLocaleTimeString()}`** (baris 120)
   - Menampilkan waktu saat ini dalam format readable
   - Contoh: "2:30:45 PM"

6. **`{" "}`** (baris 120-121)
   - Space kosong untuk formatting
   - Dalam JSX, whitespace diabaikan, jadi perlu `{" "}` untuk space

7. **`{closeHour}:00`** (baris 121)
   - Menampilkan jam tutup
   - Contoh: "22:00"

**User Experience Improvement:**
- **Buka (08:00 - 22:00)**: User melihat pesan "We're open until 22:00" dan button "Order Now" ✅
- **Tutup (23:00 - 07:59)**: Footer kosong, tidak menampilkan order section (bisa diubah dengan fallback message) ❌

### Visualisasi Flow:

**Scenario 1: Ada Pizzas (Menu)**
```
pizzaData: [pizza1, pizza2, pizza3, ...]
    ↓
numPizzas = 6
    ↓
numPizzas > 0?  → TRUE ✅
    ↓
Render: <ul className="pizzas">...</ul>
```

**Scenario 2: Tidak Ada Pizzas**
```
pizzaData: []
    ↓
numPizzas = 0
    ↓
numPizzas > 0?  → FALSE ❌
    ↓
Render: null (nothing)
```

**Scenario 3: Toko Buka (Footer)**
```
hour = 14 (2 PM)
    ↓
isOpen = 14 >= 8 && 14 <= 22  → TRUE ✅
    ↓
Render: <div className="order">...</div>
```

**Scenario 4: Toko Tutup**
```
hour = 23 (11 PM)
    ↓
isOpen = 23 >= 8 && 23 <= 22  → FALSE ❌
    ↓
Render: null (nothing)
```

### Perbedaan Ternary vs Logical AND:

```javascript
// ✅ Gunakan Ternary ketika ada dua opsi berbeda
{isOpen ? <p>Open!</p> : <p>Closed!</p>}

// ✅ Gunakan && ketika hanya show/hide (satu opsi)
{isOpen && <p>We're open, come visit us!</p>}

// ❌ JANGAN: && dengan false case yang kompleks
{isOpen && <p>Open</p> : <p>Closed</p>}  // ERROR: syntax salah

// ❌ JANGAN: gunakan || untuk render jika false
{!isOpen || <p>We're open</p>}  // confusing, gunakan ternary
```

### Common Patterns:

#### Pattern 1: Render List atau Empty State
```javascript
{numPizzas > 0 && (
  <ul className="pizzas">
    {pizzas.map(pizza => <Pizza key={pizza.name} pizzaObj={pizza} />)}
  </ul>
)}
```

#### Pattern 2: Render Action Button hanya jika Condition Met
```javascript
{isOpen && <button className="btn">Order Now</button>}
```

#### Pattern 3: Render Multiple Elements
```javascript
{isOpen && (
  <div>
    <p>We're open!</p>
    <button>Order</button>
    <p>Jam tutup: 22:00</p>
  </div>
)}
```

Parentheses `()` diperlukan ketika rendering lebih dari satu element.

#### Pattern 4: Render dengan Ternary untuk Default Message
```javascript
{isOpen ? (
  <div className="order">Come visit us!</div>
) : (
  <p>We're closed. Buka besok jam 08:00</p>
)}
```

### Troubleshooting:

**Problem 1: "false", "undefined", "null" ditampilkan**
```javascript
// ❌ SALAH: false render sebagai string
{isDone && false}     // show: "false"
{isDone && undefined} // show: "undefined"

// ✅ BENAR:
{isDone && <p>Done!</p>}  // show: JSX atau null
```

**Problem 2: Lupa parentheses untuk multiple elements**
```javascript
// ❌ ERROR:
{isOpen && (
  <p>Open</p>
  <button>Order</button>  // ERROR: more than one root element
)}

// ✅ BENAR: wrap dalam fragment atau div
{isOpen && (
  <>
    <p>Open</p>
    <button>Order</button>
  </>
)}
```

**Problem 3: Complex condition sulit dibaca**
```javascript
// ❌ Sulit dibaca:
{hour >= 8 && hour <= 22 && userLoggedIn && !hasOrdered && <button>Order</button>}

// ✅ Lebih baik: extract ke variable
const isOpen = hour >= 8 && hour <= 22;
const canOrder = userLoggedIn && !hasOrdered;
{isOpen && canOrder && <button>Order</button>}
```

---

## 6. Conditional Rendering dengan Ternary Operator (? :)

**Lokasi:** `src/index.js` baris 78-92 (Menu), baris 119-136 (Footer)

### Apa itu Ternary Operator?

Ternary operator adalah shorthand untuk if-else statement dalam JavaScript. Pattern-nya:
```javascript
condition ? expressionIfTrue : expressionIfFalse
```

Di React, ternary operator sangat powerful untuk conditional rendering karena dapat menampilkan **dua konten berbeda** berdasarkan kondisi.

### Perbandingan: && vs Ternary

#### 1. Logical AND (&&) - Hanya Show/Hide
```javascript
// Render hanya jika TRUE
{numPizzas > 0 && <ul>...</ul>}

// Jika TRUE: render <ul>
// Jika FALSE: render null (tidak ada apa-apa)
```

#### 2. Ternary (? :) - Show Different Content
```javascript
// Render dua konten berbeda
{numPizzas > 0 ? <ul>...</ul> : <p>No pizzas available</p>}

// Jika TRUE: render <ul>
// Jika FALSE: render <p>
```

**Kapan gunakan mana?**
- **&&**: Ketika hanya ingin show/hide satu elemen
- **Ternary**: Ketika ingin tampilkan sesuatu yang berbeda jika kondisi false

### A. Menu Component - Ternary untuk List vs Empty State (baris 71-95)

```javascript
function Menu () {
  const pizzas = pizzaData;
  const numPizzas = pizzas.length;

  return (
    <main className="menu">
      <h2>Our Menu</h2>
      {numPizzas > 0 ? (
        <ul className="pizzas">
          {pizzas.map((pizza) => (
            <Pizza key={pizza.name} pizzaObj={pizza} />
          ))}
        </ul>
      ) : (
        <p>We're still working on our menu. Please come back later!</p>
      )}
    </main>
  );
}
```

**Penjelasan Baris per Baris:**

1. **`{numPizzas > 0 ? (...) : (...)}`** (baris 78)
   - **Kondisi**: `numPizzas > 0` (apakah ada minimal 1 pizza?)
   - **Operator ternary**: `?` dan `:`

2. **True Case - Render List** (baris 79-89)
   ```javascript
   (
     <ul className="pizzas">
       {pizzas.map((pizza) => (
         <Pizza key={pizza.name} pizzaObj={pizza} />
       ))}
     </ul>
   )
   ```
   - Jika `numPizzas > 0`: Tampil list dengan semua pizzas
   - Parentheses diperlukan karena lebih dari satu baris

3. **False Case - Render Fallback Message** (baris 90-92)
   ```javascript
   : (
     <p>We're still working on our menu. Please come back later!</p>
   )
   ```
   - Jika `numPizzas === 0`: Tampil pesan friendly
   - User tahu menu sedang di-develop, bukan bug

**User Experience Improvement:**
- ✅ Dengan pizzas: User melihat menu yang menarik
- ✅ Tanpa pizzas: User mendapat pesan yang helpful, bukan halaman kosong yang bingung

### B. Footer Component - Ternary untuk Open vs Closed (baris 110-139)

```javascript
function Footer () {
  const hour = new Date().getHours()
  const openHour = 12;
  const closeHour = 22
  const isOpen = hour >= openHour && hour <= closeHour

  return (
    <footer className="footer">
      {isOpen ? (
        <div className="order">
          <p>
            {new Date().toLocaleTimeString()}. We're currently open until{" "}
            {closeHour}:00. Come visit us or order online.
          </p>
          <button className="btn">Order Now</button>
          <p>© 2023 Fast React Pizza Co. All rights reserved.</p>
        </div>
      ) : (
        <div className="order">
          <p>
            We're closed right now. Come back between {openHour}:00 and{" "}
            {closeHour}:00.
          </p>
          <p>© 2023 Fast React Pizza Co. All rights reserved.</p>
        </div>
      )}
    </footer>
  );
}
```

**Penjelasan Baris per Baris:**

1. **`{isOpen ? (...) : (...)}`** (baris 119)
   - **Kondisi**: `isOpen` (apakah toko sedang buka?)
   - Derived dari: `hour >= 12 && hour <= 22` (jam 12:00 - 22:00)

2. **True Case - Open Content** (baris 120-127)
   ```javascript
   (
     <div className="order">
       <p>{new Date().toLocaleTimeString()}. We're currently open until {closeHour}:00...</p>
       <button className="btn">Order Now</button>
       <p>© 2023 Fast React Pizza Co. All rights reserved.</p>
     </div>
   )
   ```
   - Menampilkan waktu saat ini dengan `.toLocaleTimeString()`
   - Pesan yang menginvite user untuk order
   - Button "Order Now" untuk call-to-action
   - Copyright info

3. **False Case - Closed Content** (baris 128-136)
   ```javascript
   : (
     <div className="order">
       <p>We're closed right now. Come back between {openHour}:00 and {closeHour}:00.</p>
       <p>© 2023 Fast React Pizza Co. All rights reserved.</p>
     </div>
   )
   ```
   - Menampilkan jam operasional (12:00 - 22:00)
   - Friendly message, bukan hanya kosong
   - Tidak ada button "Order Now" karena belum buka

**User Experience Improvement:**
- ✅ **Jam 14:00 (Buka)**: User melihat "We're currently open until 22:00" + Order button → encourage action
- ✅ **Jam 23:00 (Tutup)**: User melihat "Come back between 12:00 and 22:00" → set expectations

### Visualisasi Ternary Operator:

```
Condition Check
    ↓
isOpen?
    ├─ TRUE  → Render "Open" content
    └─ FALSE → Render "Closed" content

Condition Check
    ↓
numPizzas > 0?
    ├─ TRUE  → Render List
    └─ FALSE → Render "Empty State" message
```

### Syntax Variations:

#### Single Line (Simple)
```javascript
{isOpen ? <p>Open!</p> : <p>Closed!</p>}
```

#### Multi-line (Readable)
```javascript
{isOpen ? (
  <div className="order">
    <p>We're open!</p>
    <button>Order</button>
  </div>
) : (
  <p>We're closed</p>
)}
```

#### With JSX Fragments
```javascript
{isOpen ? (
  <>
    <p>Open</p>
    <button>Order</button>
  </>
) : (
  <>
    <p>Closed</p>
    <p>Opening tomorrow at 12:00</p>
  </>
)}
```

### Nesting Ternary (Gunakan dengan Hati-hati!)

Ternary dapat di-nest, tapi bisa sulit dibaca:

```javascript
// ❌ JANGAN: Terlalu complicated
{isOpen ? (
  numPizzas > 0 ? (
    <ul>...</ul>
  ) : (
    <p>No pizzas</p>
  )
) : (
  <p>Closed</p>
)}

// ✅ BAIK: Extract ke variable terlebih dahulu
const hasMenu = numPizzas > 0;
const isCurrentlyOpen = isOpen;

{isCurrentlyOpen ? (
  hasMenu ? (
    <ul>...</ul>
  ) : (
    <p>No pizzas</p>
  )
) : (
  <p>Closed</p>
)}

// ✅ ATAU: Render di parent, bukan nested
function MenuContent() {
  return numPizzas > 0 ? <ul>...</ul> : <p>No pizzas</p>;
}

function Menu() {
  return <main>{isOpen && <MenuContent />}</main>;
}
```

### Common Patterns dengan Ternary:

#### Pattern 1: List or Empty State
```javascript
{pizzas.length > 0 ? (
  <ul>
    {pizzas.map(p => <Pizza key={p.name} pizzaObj={p} />)}
  </ul>
) : (
  <p>No items available</p>
)}
```

#### Pattern 2: Loading State
```javascript
{isLoading ? (
  <p>Loading...</p>
) : (
  <div>Content loaded!</div>
)}
```

#### Pattern 3: Auth State
```javascript
{isLoggedIn ? (
  <UserProfile />
) : (
  <LoginForm />
)}
```

#### Pattern 4: Based on Data
```javascript
{user.isAdmin ? (
  <AdminPanel />
) : (
  <UserDashboard />
)}
```

### Troubleshooting Ternary:

**Problem 1: Syntax Error - Lupa Parentheses**
```javascript
// ❌ ERROR:
{isOpen ?
  <p>Open</p>
  <button>Order</button>  // ERROR
:
  <p>Closed</p>
}

// ✅ BENAR:
{isOpen ? (
  <>
    <p>Open</p>
    <button>Order</button>
  </>
) : (
  <p>Closed</p>
)}
```

**Problem 2: Tidak bisa render conditional rendering**
```javascript
// ❌ SALAH: JSX expression harus berupa element
{isOpen ? <p>Hello</p> : undefined}  // Show "undefined" text

// ✅ BENAR:
{isOpen ? <p>Hello</p> : null}  // Show nothing

// ✅ ATAU dengan &&:
{isOpen && <p>Hello</p>}
```

**Problem 3: Complex ternary jadi hard to read**
```javascript
// ❌ Sulit dibaca - banyak nested ternary

// ✅ Solusi: Extract ke function atau variable
const handleRender = () => {
  if (isOpen && hasMenu) return <MenuList />;
  if (isOpen && !hasMenu) return <NoMenu />;
  return <ClosedMessage />;
};

{handleRender()}
```

---

## 7. Destructuring Props - Cleaner Code Syntax

**Lokasi:** `src/index.js` baris 97 (Pizza), baris 136 (Order)

### Apa itu Destructuring?

Destructuring adalah cara untuk extract nilai dari object dan assign ke variable secara langsung. Di React, ini membuat component code lebih clean dan readable.

### Sebelum vs Sesudah Destructuring:

#### ❌ Sebelum (Menggunakan props object)
```javascript
function Pizza(props) {
  return (
    <li className="pizza">
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

**Problem:**
- Perlu menulis `props.pizzaObj.` berulang kali
- Sulit dibaca, noisy dengan repetisi

#### ✅ Sesudah (Dengan Destructuring)
```javascript
function Pizza({ pizzaObj }) {
  return (
    <li className="pizza">
      <img src={pizzaObj.photoName} alt={pizzaObj.name}/>
      <div>
        <h3>{pizzaObj.name}</h3>
        <p>{pizzaObj.ingredients}</p>
        <span>${pizzaObj.price}</span>
      </div>
    </li>
  )
}
```

**Keuntungan:**
- Langsung akses `pizzaObj` tanpa prefix `props.`
- Code lebih clean dan readable
- Jelas apa props yang digunakan

### A. Pizza Component - Destructuring dalam Parameter (baris 97-110)

```javascript
function Pizza({ pizzaObj }) {
  if (pizzaObj.soldOut) return null;

  return (
    <li className="pizza">
      <img src={pizzaObj.photoName} alt={pizzaObj.name} />
      <div>
        <h3>{pizzaObj.name}</h3>
        <p>{pizzaObj.ingredients}</p>
        <span>${pizzaObj.price}</span>
      </div>
    </li>
  );
}
```

**Penjelasan Baris per Baris:**

1. **`function Pizza({ pizzaObj })`** (baris 97)
   - **Destructuring di parameter**: Langsung extract `pizzaObj` dari props
   - Equivalent dengan: `function Pizza(props) { const { pizzaObj } = props; }`
   - Lebih singkat dan langsung

2. **`if (pizzaObj.soldOut) return null;`** (baris 98)
   - **Early Return Pattern**: Return early jika kondisi terpenuhi
   - Jika `pizzaObj.soldOut === true`: return `null` (render nothing)
   - Jika `pizzaObj.soldOut === false`: lanjut ke code berikutnya
   - **Benefit**: Menghindari nested JSX, code lebih linear

3. **Menggunakan `pizzaObj` langsung** (baris 102-106)
   - `pizzaObj.photoName`, `pizzaObj.name`, dll
   - Tidak perlu `props.pizzaObj.name` lagi

**Contoh Data Flow:**

```
Menu Component:
  <Pizza key={pizza.name} pizzaObj={pizza} />

Pizza Component:
  Props yang dikirim: { pizzaObj: { name: "Pizza Salamino", soldOut: true, ... } }

  Destructuring:
  function Pizza({ pizzaObj }) {
    // pizzaObj = { name: "Pizza Salamino", soldOut: true, ... }
  }

  Early Return:
  if (pizzaObj.soldOut) return null;  // Return null karena soldOut: true
```

### B. Order Component - Destructuring Multiple Props (baris 136-147)

Komponen baru yang di-extract dari Footer untuk better composition:

```javascript
function Order({ close }) {
  return (
    <div className="order">
      <p>
        {new Date().toLocaleTimeString()}. We're currently open until {close}
        :00. Come visit us or order online.
      </p>
      <button className="btn">Order Now</button>
      <p>© 2023 Fast React Pizza Co. All rights reserved.</p>
    </div>
  );
}
```

**Penjelasan:**

1. **`function Order({ close })`** (baris 136)
   - Destructuring satu prop `close` dari parameter
   - Ini adalah shorthand untuk `function Order(props) { const { close } = props; }`
   - Prop ini dikirim dari Footer: `<Order close={closeHour} />`

2. **Menggunakan `close` langsung** (baris 140)
   - `{close}:00` menampilkan jam tutup
   - Contoh: jika `closeHour = 22`, render "22:00"

**Contoh Data Flow:**

```
Footer Component:
  const closeHour = 22
  <Order close={closeHour} />

Order Component:
  Props yang dikirim: { close: 22 }

  Destructuring:
  function Order({ close }) {
    // close = 22
  }

  Render:
  "We're currently open until 22:00"
```

### Variasi Destructuring:

#### 1. Single Prop
```javascript
function Order({ close }) {
  return <p>Until {close}:00</p>
}

// Usage:
<Order close={22} />
```

#### 2. Multiple Props
```javascript
function Pizza({ name, price, ingredients }) {
  return (
    <div>
      <h3>{name}</h3>
      <p>{ingredients}</p>
      <span>${price}</span>
    </div>
  )
}

// Usage:
<Pizza name="Margherita" price={10} ingredients="Tomato and mozzarella" />
```

#### 3. Object Prop
```javascript
function Pizza({ pizzaObj }) {
  return (
    <div>
      <h3>{pizzaObj.name}</h3>
      <span>${pizzaObj.price}</span>
    </div>
  )
}

// Usage:
<Pizza pizzaObj={{ name: "Margherita", price: 10 }} />
```

#### 4. Deep Destructuring (Dari nested object)
```javascript
function Pizza({ pizzaObj: { name, price } }) {
  return (
    <div>
      <h3>{name}</h3>
      <span>${price}</span>
    </div>
  )
}

// Usage:
<Pizza pizzaObj={{ name: "Margherita", price: 10 }} />
// Langsung akses name dan price tanpa pizzaObj.name
```

#### 5. Default Values
```javascript
function Order({ close = 22 }) {
  return <p>Until {close}:00</p>
}

// Jika tidak ada prop close, default ke 22
<Order />  // akan render "Until 22:00"
```

### Early Return Pattern - Kenapa Penting?

**❌ Tanpa Early Return (Nested):**
```javascript
function Pizza({ pizzaObj }) {
  return (
    <li>
      {!pizzaObj.soldOut ? (
        <div>
          <h3>{pizzaObj.name}</h3>
          <p>{pizzaObj.ingredients}</p>
          <span>${pizzaObj.price}</span>
        </div>
      ) : null}
    </li>
  )
}
```
- Nested ternary membuat code sulit dibaca
- JSX structure kompleks

**✅ Dengan Early Return (Linear):**
```javascript
function Pizza({ pizzaObj }) {
  if (pizzaObj.soldOut) return null;

  return (
    <li>
      <h3>{pizzaObj.name}</h3>
      <p>{pizzaObj.ingredients}</p>
      <span>${pizzaObj.price}</span>
    </li>
  )
}
```
- Code flow lebih linear
- Mudah dibaca dan maintain
- Early exit untuk edge cases

### Kapan Gunakan Destructuring:

✅ **Gunakan Destructuring:**
- Ketika props sering diakses (lebih dari 2-3 kali)
- Untuk membuat code lebih readable
- Standard practice di React modern

❌ **Tidak perlu Destructuring:**
- Hanya mengakses 1-2 props
- Props dinamis (tidak tahu nama prop sebelumnya)

```javascript
// OK tanpa destructuring jika props minimal:
function Header(props) {
  return <h1>{props.title}</h1>
}

// Baik dengan destructuring jika props banyak:
function Pizza({ pizzaObj }) {
  // mengakses pizzaObj.name, pizzaObj.price, dll
}
```

### Comparison: Component Composition Improvement

**Sebelum (Semua di Footer):**
```javascript
function Footer () {
  const hour = new Date().getHours()
  const isOpen = hour >= 12 && hour <= 22

  return (
    <footer>
      {isOpen ? (
        <div className="order">
          <p>{new Date().toLocaleTimeString()}. We're currently open until 22:00...</p>
          <button className="btn">Order Now</button>
          <p>© 2023 Fast React Pizza Co. All rights reserved.</p>
        </div>
      ) : (
        <div className="order">
          <p>We're closed right now. Come back between 12:00 and 22:00.</p>
          <p>© 2023 Fast React Pizza Co. All rights reserved.</p>
        </div>
      )}
    </footer>
  );
}
```

**Sesudah (Terpisah menjadi Order component):**
```javascript
function Order({ close }) {
  return (
    <div className="order">
      <p>{new Date().toLocaleTimeString()}. We're currently open until {close}:00...</p>
      <button className="btn">Order Now</button>
      <p>© 2023 Fast React Pizza Co. All rights reserved.</p>
    </div>
  );
}

function Footer () {
  const hour = new Date().getHours()
  const isOpen = hour >= 12 && hour <= 22

  return (
    <footer className="footer">
      {isOpen ? (
        <Order close={22} />
      ) : (
        <div className="order">
          <p>We're closed right now. Come back between 12:00 and 22:00.</p>
          <p>© 2023 Fast React Pizza Co. All rights reserved.</p>
        </div>
      )}
    </footer>
  );
}
```

**Keuntungan:**
- Order component bisa dipakai di tempat lain (reusable)
- Footer component lebih singkat dan fokus
- Setiap component punya satu responsibility
- Lebih mudah di-test

---

## 8. Macam-macam Cara Menambahkan Styling di React

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

## 9. Array Data dan Map (Future Development)

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
6. ✅ **Destructuring Props**: Extract props langsung di function parameter untuk cleaner code
7. ✅ **Dynamic Content**: Menggunakan JavaScript expressions dalam JSX dengan curly braces
8. ✅ **Array.map()**: Transform array menjadi multiple React components
9. ✅ **Key Attribute**: Unique identifier untuk list items (penting untuk React performance)
10. ✅ **Semantic HTML**: Menggunakan `<ul>`, `<li>` untuk list items
11. ✅ **Conditional Rendering (&&)**: Render element hanya jika kondisi TRUE dengan logical AND operator
12. ✅ **Conditional Rendering (Ternary ? :)**: Render dua konten berbeda berdasarkan kondisi
13. ✅ **Early Return Pattern**: Return early untuk edge cases, membuat code lebih linear
14. ✅ **Fallback Messages**: Menampilkan helpful message ketika kondisi tidak terpenuhi
15. ✅ **Conditional Logic**: JavaScript logic dalam components (variable, if, &&, ternary)
16. ✅ **CSS Classes**: Styling components dengan external CSS file
17. ✅ **Flexbox & Grid**: Layout dengan modern CSS
18. ✅ **React.StrictMode**: Development tool untuk mendeteksi bugs

---

## Tips untuk Pengembangan Lebih Lanjut

### 1. ✅ Rendering Multiple Pizzas (SUDAH IMPLEMENTED)
Sekarang menggunakan `pizzaData.map()` untuk render semua 6 pizzas dari array secara dinamis dengan semantic `<ul>` dan `<li>` tags.

**Next Step:** Tambahkan filter atau search untuk filter pizzas berdasarkan ingredient atau price range.

### 2. ✅ Conditional Rendering - Show/Hide & Fallback (SUDAH IMPLEMENTED)
Sekarang menggunakan **ternary operator (? :)** untuk tampilkan konten yang berbeda:

**Menu Component - List or Empty State:**
```javascript
{numPizzas > 0 ? (
  <ul className="pizzas">
    {pizzas.map((pizza) => (
      <Pizza key={pizza.name} pizzaObj={pizza} />
    ))}
  </ul>
) : (
  <p>We're still working on our menu. Please come back later!</p>
)}
```
- Jika ada pizzas: Tampil list
- Jika kosong: Tampil helpful fallback message

**Footer Component - Open or Closed:**
```javascript
{isOpen ? (
  <div className="order">
    <p>{new Date().toLocaleTimeString()}. We're currently open until {closeHour}:00...</p>
    <button className="btn">Order Now</button>
  </div>
) : (
  <div className="order">
    <p>We're closed right now. Come back between {openHour}:00 and {closeHour}:00.</p>
  </div>
)}
```
- Jika buka (12:00-22:00): Tampil jam + invite untuk order
- Jika tutup: Tampil jam operasional

**Key Improvement:** Kedua komponen sekarang punya fallback yang user-friendly, bukan hanya kosong/blank.

### 3. ✅ Conditional CSS Classes + Early Return (SUDAH IMPLEMENTED)
Pizza component sekarang menggunakan early return untuk hide sold-out items:

```javascript
function Pizza({ pizzaObj }) {
  if (pizzaObj.soldOut) return null;  // Early return

  return (
    <li className="pizza">
      <img src={pizzaObj.photoName} alt={pizzaObj.name} />
      <div>
        <h3>{pizzaObj.name}</h3>
        <p>{pizzaObj.ingredients}</p>
        <span>${pizzaObj.price}</span>
      </div>
    </li>
  );
}
```

**Improvement:**
- Early return untuk handle soldOut case
- Code lebih linear (tidak nested)
- Sold-out items tidak render di list

### 4. ✅ Destructuring Props - Cleaner Code (SUDAH IMPLEMENTED)
Sekarang menggunakan destructuring di function parameter:

**Pizza Component:**
```javascript
function Pizza({ pizzaObj }) {
  // Direct akses: pizzaObj.name, pizzaObj.price, dll
  return <h3>{pizzaObj.name}</h3>
}
```

**Order Component (Baru):**
```javascript
function Order({ close }) {
  return <p>Until {close}:00</p>
}

// Digunakan di Footer:
<Order close={closeHour} />
```

**Keuntungan Destructuring:**
- Cleaner, tidak perlu `props.` prefix
- Jelas apa props yang digunakan
- Standard practice di React modern

### 5. ✅ Component Composition & Reusable Components (SUDAH IMPLEMENTED)
Order component di-extract dari Footer untuk reusability:

**Sebelum (Order inline di Footer):**
```javascript
function Footer() {
  return (
    <footer>
      {isOpen ? (
        <div className="order">
          <p>{new Date().toLocaleTimeString()}. We're open until 22:00...</p>
          <button className="btn">Order Now</button>
        </div>
      ) : (...)}
    </footer>
  )
}
```

**Sesudah (Order sebagai component terpisah):**
```javascript
function Order({ close }) {
  return (
    <div className="order">
      <p>We're open until {close}:00...</p>
      <button className="btn">Order Now</button>
    </div>
  )
}

function Footer() {
  return (
    <footer>
      {isOpen ? <Order close={closeHour} /> : ...}
    </footer>
  )
}
```

**Benefits:**
- Order component reusable di tempat lain
- Footer lebih clean dan fokus
- Single responsibility principle
- Lebih mudah di-test

### 6. State Management dengan useState (Untuk Future)
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

### 7. Spread Operator untuk Props
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
