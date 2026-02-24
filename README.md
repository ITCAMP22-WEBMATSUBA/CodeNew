# Svelte & Svelte kit

## Logic Blocks

### If Blocks
```Svelte
<script>
  let score = $state(0);
</script>

<h2>Grade Calculator</h2>
<input type="number" bind:value={score} min="0" max="100" />

{#if score >= 80}
  <p>Grade: A 🏆</p>
{:else if score >= 70}
  <p>Grade: B 👍</p>
{:else if score >= 60}
  <p>Grade: C 🙂</p>
{:else if score >= 50}
  <p>Grade: D 😬</p>
{:else}
  <p>Grade: F 💀</p>
{/if}
```

### Each Blocks
```Svelte
<script>
  let students_data = $state([ // ข้อมูล Array
    { id: 1, name: "Anna", score: 82 },
    { id: 2, name: "Ben", score: 74 },
    { id: 3, name: "Chris", score: 91 }
  ]);
</script>

<!-- เริ่มการลูป -->
{#each students_data as student}
  <div>
    <p>Name: {student.name}</p>
    <p>Score: {student.score}</p>
    <hr>
  </div>
{/each}

```

### Await Blocks
``` Svelte
<script>
  async function getUser() { // ขอข้อมูลจาก Backend ผ่าน API
    const res = await fetch('/api/user');
    if (!res.ok) throw new Error('Failed to fetch');
    return res.json();
  }
  let promise = getUser();
</script>

{#await promise}
  <p>...กำลังโหลดข้อมูล...</p>
{:then user}
  <h1>สวัสดี, {user.name}</h1>
{:catch error}
  <p style="color: red">เกิดข้อผิดพลาด: {error.message}</p>
{/await}
```

## Events

### onclick
```Svelte
<script>
	let count = $state(0);

	function increment() {
		count += 1;
	}
</script>

<button onclick={increment}>
	กดฉัน: {count}
</button>

<button onclick={() => count = 0}>
	รีเซ็ต
</button>
```

### onclick (once)
```Svelte
<script>
  //import once เข้ามาก่อน
  import { once } from "svelte/legacy"; 

  let voteCount = $state(0);
  let message = $state("กรุณากดโหวต");

  function handleVote() {
    voteCount += 1;
    message = "ขอบคุณที่โหวต! (คุณใช้สิทธิ์ไปแล้ว)";
    alert("บันทึกคะแนนเรียบร้อย!");
  }
</script>

<h1>คะแนนรวม: {voteCount}</h1>
<p>{message}</p>

<button onclick={once(handleVote)}>
  🗳️ กดโหวต (กดได้ครั้งเดียว)
</button>

```

## Data Binding

### value
```Svelte
<script>
  let username = $state("Camper IT Camp22");
</script>

<div class="box">
  <input type="text" bind:value={username} placeholder="ชื่อผู้ใช้" />
  <p>สวัสดีน้อง: <strong>{username}</strong></p>
</div>
```
### checked
```Svelte
<script>
  let isReady = $state(false);
</script>

<div class="box">
  <label>
    <input type="checkbox" bind:checked={isReady} />
    ฉันพร้อมจะลุยแล้ว
  </label>

  {#if isReady}
    <p style="color: green;">ลุยกันเลย!</p>
  {:else}
    <p style="color: red;">กรุณาติ๊กถูกก่อน</p>
  {/if}
</div>
```
### group
```Svelte
<script>
  let theme = $state("light"); // ค่าเริ่มต้น
</script>

<div class="box">
  <p>โหมดปัจจุบัน: <strong>{theme}</strong></p>

  <label>
    <input type="radio" bind:group={theme} value="light" />
    ☀️ Light Mode
  </label>

  <label>
    <input type="radio" bind:group={theme} value="dark" />
    🌙 Dark Mode
  </label>
</div>
```

## Components

### ตัวอย่างเรียกใช้ components
``` Svelte
<script>
    // import เข้ามาโดยชื่อ Component ต้องขึ้นต้นด้วยตัวพิมพ์ใหญ่เสมอ)
    import Navbar from "$lib/components/Navbar.svelte";
</script>

<!-- import ครั้งเดียวเอามาแปะใช้กี่ครั้งก็ได้ -->
<Navbar />
<Navbar />
<Navbar />

<h1>Welcome to IT camp22</h1>
```

## Props
```Svelte
<script>
	let { name } = $props();
</script>

<div>
	สวัสดี! ฉันชื่อ: <strong>{name}</strong>
</div>
```

### เรียกใช้ Component และ props ค่ามา
```Svelte
<script>
  import Profile from "$lib/components/Profile.svelte";
</script>

<Profile name="สมชาย" />

<Profile name="สมหญิง" />

<Profile name="Tony" />
```

### พยายามผูกค่า Props โดยไม่ใช้ $bindable()
Profile.svelte
```Svelte
<script>
	let { name } = $props();

	function changeName() {
		name = "Somchai"
	}
</script>

<p>Child name value: {name}</p>
<button onclick={changeName}>เปลี่ยนชื่อเป็น Somchai</button>
```
App.svelte
```Svelte
<script>
	import Profile from "./Profile.svelte";

	let currentName = $state("Somsak");
</script>

<p>Parent name value: {name}</p>
<Profile name={currentName} />
```

### ผูกค่า Props โดยใช้ $bindable()
Profile.svelte
```Svelte
<script>
	let { name= $bindable() } = $props();

	function changeName() {
		name = "Somchai"
	}
</script>

<p>Child name value: {name}</p>
<button onclick={changeName}>เปลี่ยนชื่อเป็น Somchai</button>
```
App.svelte
```Svelte
<script>
	import Profile from "./Profile.svelte";

	let currentName = $state("Somsak");
</script>

<p>Parent name value: {name}</p>
<Profile bind:name={currentName} />
```

