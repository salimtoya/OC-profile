<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ข้อมูล OC - Archives & World Compendium</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>
  <!-- Google Fonts (Sarabun & Mitr) -->
  <link href="https://fonts.googleapis.com/css2?family=Mitr:wght@400;500;600&family=Sarabun:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            warmBg: '#FAF7F2',
            warmCard: '#FFFFFF',
            warmBorder: '#EFEAE1',
            warmPrimary: '#7C6A58',
            warmPrimaryHover: '#635343',
            warmAccent: '#D97706',
            warmMuted: '#8C827A'
          },
          fontFamily: {
            sans: ['Sarabun', 'sans-serif'],
            heading: ['Mitr', 'sans-serif']
          }
        }
      }
    }
  </script>
  <style>
    body { background-color: #FAF7F2; color: #4A4036; font-family: 'Sarabun', sans-serif; }
    .heading { font-family: 'Mitr', sans-serif; }
    .hide-scrollbar::-webkit-scrollbar { display: none; }
    .hide-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
  </style>
</head>
<body class="min-h-screen flex flex-col">

  <!-- Header / Navigation -->
  <header class="bg-white/90 backdrop-blur-md border-b border-warmBorder sticky top-0 z-40">
    <div class="max-w-7xl mx-auto px-4 py-3 flex items-center justify-between">
      <div class="flex items-center space-x-3">
        <div class="w-10 h-10 rounded-xl bg-warmPrimary text-white flex items-center justify-center font-bold text-xl shadow-sm">
          OC
        </div>
        <div>
          <h1 class="heading font-semibold text-lg text-warmPrimary leading-tight">ข้อมูล OC</h1>
          <p class="text-xs text-warmMuted">คลังเก็บข้อมูลตัวละครและโลกความสัมพันธ์</p>
        </div>
      </div>

      <!-- Auth State Info -->
      <div id="authHeaderState" class="flex items-center space-x-3">
        <!-- JS จะเติมข้อมูลผู้ใช้/ปุ่มล็อกอินตรงนี้ -->
      </div>
    </div>
  </header>

  <!-- Main Content Area -->
  <main class="flex-1 max-w-7xl w-full mx-auto p-4 md:p-6">
    
    <!-- Auth Screen (สำหรับเข้าสู่ระบบ/สมัครสมาชิก) -->
    <div id="authScreen" class="max-w-md mx-auto my-12 bg-white p-8 rounded-2xl border border-warmBorder shadow-sm">
      <div class="text-center mb-6">
        <div class="w-16 h-16 bg-amber-50 text-warmAccent rounded-2xl flex items-center justify-center mx-auto mb-3">
          <i data-lucide="book-open" class="w-8 h-8"></i>
        </div>
        <h2 id="authTitle" class="heading text-2xl font-semibold text-warmPrimary mb-2">เข้าสู่ระบบ "ข้อมูล OC"</h2>
        <p class="text-sm text-warmMuted">เข้าถึงคลังข้อมูล OC ของคุณได้จากทุกอุปกรณ์</p>
      </div>

      <form id="authForm" class="space-y-4">
        <div>
          <label class="block text-xs font-semibold text-warmMuted uppercase mb-1">อีเมล</label>
          <input type="email" id="authEmail" required class="w-full px-4 py-2.5 rounded-xl border border-warmBorder focus:outline-none focus:border-warmPrimary text-sm bg-warmBg/50" placeholder="your@email.com">
        </div>
        <div>
          <label class="block text-xs font-semibold text-warmMuted uppercase mb-1">รหัสผ่าน (อย่างน้อย 6 ตัวอักษร)</label>
          <input type="password" id="authPassword" required minlength="6" class="w-full px-4 py-2.5 rounded-xl border border-warmBorder focus:outline-none focus:border-warmPrimary text-sm bg-warmBg/50" placeholder="••••••••">
        </div>

        <div id="authError" class="text-xs text-red-500 hidden bg-red-50 p-3 rounded-lg border border-red-100"></div>

        <button type="submit" id="authSubmitBtn" class="w-full py-3 bg-warmPrimary hover:bg-warmPrimaryHover text-white rounded-xl font-medium text-sm transition-colors shadow-sm">
          เข้าสู่ระบบ
        </button>
      </form>

      <div class="mt-6 pt-6 border-t border-warmBorder text-center">
        <button id="toggleAuthModeBtn" class="text-sm text-warmAccent hover:underline font-medium">
          ยังไม่มีบัญชี? สมัครสมาชิกใหม่ที่นี่
        </button>
      </div>
    </div>

    <!-- App Dashboard (แสดงเมื่อเข้าสู่ระบบแล้ว) -->
    <div id="appDashboard" class="hidden space-y-6">
      
      <!-- Top Navigation Tabs & Controls -->
      <div class="flex flex-wrap items-center justify-between gap-4 bg-white p-4 rounded-2xl border border-warmBorder shadow-sm">
        <div class="flex items-center space-x-2 overflow-x-auto pb-1 md:pb-0 w-full md:w-auto">
          <button id="tabCharsBtn" onclick="switchTab('chars')" class="px-4 py-2 rounded-xl text-sm font-medium bg-warmPrimary text-white shadow-sm whitespace-nowrap">
            <i data-lucide="users" class="w-4 h-4 inline mr-1.5"></i> รายชื่อ OC
          </button>
          <button id="tabWorldsBtn" onclick="switchTab('worlds')" class="px-4 py-2 rounded-xl text-sm font-medium text-warmMuted hover:bg-warmBg whitespace-nowrap">
            <i data-lucide="globe" class="w-4 h-4 inline mr-1.5"></i> โลก / จักรวาล
          </button>
          <button id="tabRelBtn" onclick="switchTab('rel')" class="px-4 py-2 rounded-xl text-sm font-medium text-warmMuted hover:bg-warmBg whitespace-nowrap">
            <i data-lucide="git-fork" class="w-4 h-4 inline mr-1.5"></i> แผนผังความสัมพันธ์
          </button>
          <button id="tabCompareBtn" onclick="switchTab('compare')" class="px-4 py-2 rounded-xl text-sm font-medium text-warmMuted hover:bg-warmBg whitespace-nowrap">
            <i data-lucide="columns" class="w-4 h-4 inline mr-1.5"></i> เปรียบเทียบ OC
          </button>
        </div>

        <div class="flex items-center space-x-2 w-full md:w-auto justify-end">
          <button id="openAddCharModalBtn" onclick="openCharModal()" class="px-4 py-2 bg-warmAccent hover:bg-amber-600 text-white rounded-xl text-sm font-medium transition-colors shadow-sm flex items-center">
            <i data-lucide="plus" class="w-4 h-4 mr-1.5"></i> เพิ่ม OC ใหม่
          </button>
        </div>
      </div>

      <!-- Filter Bar for Characters -->
      <div id="charFilterBar" class="flex flex-wrap items-center justify-between gap-3 bg-white/60 p-3 rounded-xl border border-warmBorder">
        <div class="flex items-center space-x-3 w-full sm:w-auto">
          <span class="text-xs font-semibold text-warmMuted uppercase">กรองตามโลก:</span>
          <select id="worldFilterSelect" onchange="renderCharacters()" class="px-3 py-1.5 rounded-lg border border-warmBorder text-sm bg-white focus:outline-none">
            <option value="ALL">ทั้งหมด (ทุกจักรวาล)</option>
          </select>
        </div>
        <div class="text-xs text-warmMuted" id="charCountLabel">กำลังโหลดข้อมูล...</div>
      </div>

      <!-- Character List Section -->
      <section id="charsSection" class="space-y-4">
        <div id="charGrid" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
          <!-- OC Cards จะถูกแสดงตรงนี้ -->
        </div>
      </section>

      <!-- World Section -->
      <section id="worldsSection" class="hidden space-y-4">
        <div class="flex justify-between items-center bg-white p-4 rounded-xl border border-warmBorder">
          <div>
            <h3 class="heading text-lg font-semibold text-warmPrimary">โลก / จักรวาลทั้งหมด</h3>
            <p class="text-xs text-warmMuted">จัดการจักรวาลและแยกกลุ่ม OC ตามเรื่องราว</p>
          </div>
          <button onclick="openWorldModal()" class="px-3 py-1.5 bg-warmPrimary text-white rounded-lg text-sm font-medium hover:bg-warmPrimaryHover">
            + เพิ่มโลกใหม่
          </button>
        </div>
        <div id="worldGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
          <!-- World Cards -->
        </div>
      </section>

      <!-- Relationship Canvas Section -->
      <section id="relSection" class="hidden space-y-4">
        <div class="bg-white p-4 rounded-xl border border-warmBorder flex flex-wrap justify-between items-center gap-2">
          <div>
            <h3 class="heading text-lg font-semibold text-warmPrimary">แผนผังความสัมพันธ์</h3>
            <p class="text-xs text-warmMuted">ลากและวางโหนด OC เพื่อจัดตำแหน่งความสัมพันธ์ตามใจชอบ</p>
          </div>
          <div class="flex items-center space-x-2">
            <button onclick="openAddRelModal()" class="px-3 py-1.5 bg-warmAccent text-white rounded-lg text-sm font-medium hover:bg-amber-600">
              + เพิ่มความสัมพันธ์
            </button>
          </div>
        </div>

        <div id="canvasContainer" class="relative w-full h-[550px] bg-white rounded-2xl border border-warmBorder overflow-hidden shadow-inner touch-none">
          <svg id="relSvg" class="absolute inset-0 w-full h-full pointer-events-none"></svg>
          <div id="nodesContainer" class="absolute inset-0"></div>
        </div>
      </section>

      <!-- Compare Section -->
      <section id="compareSection" class="hidden space-y-4">
        <div class="bg-white p-4 rounded-xl border border-warmBorder">
          <h3 class="heading text-lg font-semibold text-warmPrimary mb-3">เปรียบเทียบข้อมูลตัวละคร</h3>
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div>
              <label class="block text-xs font-semibold text-warmMuted uppercase mb-1">เลือก OC ตัวที่ 1</label>
              <select id="compareChar1" onchange="renderCompare()" class="w-full p-2.5 rounded-xl border border-warmBorder text-sm"></select>
            </div>
            <div>
              <label class="block text-xs font-semibold text-warmMuted uppercase mb-1">เลือก OC ตัวที่ 2</label>
              <select id="compareChar2" onchange="renderCompare()" class="w-full p-2.5 rounded-xl border border-warmBorder text-sm"></select>
            </div>
          </div>
        </div>
        <div id="compareResult" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>
      </section>

    </div>
  </main>

  <!-- Modal: View OC Full Profile -->
  <div id="viewCharModal" class="fixed inset-0 bg-black/40 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
    <div class="bg-white max-w-2xl w-full rounded-2xl max-h-[90vh] overflow-y-auto p-6 border border-warmBorder shadow-xl relative">
      <button onclick="closeModal('viewCharModal')" class="absolute top-4 right-4 p-2 text-warmMuted hover:text-warmPrimary">
        <i data-lucide="x" class="w-5 h-5"></i>
      </button>
      <div id="viewCharContent"></div>
    </div>
  </div>

  <!-- Modal: Add / Edit OC -->
  <div id="charModal" class="fixed inset-0 bg-black/40 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
    <div class="bg-white max-w-xl w-full rounded-2xl max-h-[90vh] overflow-y-auto p-6 border border-warmBorder shadow-xl relative">
      <button onclick="closeModal('charModal')" class="absolute top-4 right-4 p-2 text-warmMuted hover:text-warmPrimary">
        <i data-lucide="x" class="w-5 h-5"></i>
      </button>
      <h3 id="charModalTitle" class="heading text-xl font-semibold text-warmPrimary mb-4">เพิ่มข้อมูล OC ใหม่</h3>
      
      <form id="charForm" class="space-y-4">
        <input type="hidden" id="charId">
        
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
          <div>
            <label class="block text-xs font-semibold text-warmMuted uppercase mb-1">ชื่อ OC</label>
            <input type="text" id="charName" required class="w-full p-2.5 rounded-xl border border-warmBorder text-sm" placeholder="เช่น เอลเลน">
          </div>
          <div>
            <label class="block text-xs font-semibold text-warmMuted uppercase mb-1">สังกัดโลก / จักรวาล</label>
            <select id="charWorldId" class="w-full p-2.5 rounded-xl border border-warmBorder text-sm"></select>
          </div>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
          <div>
            <label class="block text-xs font-semibold text-warmMuted uppercase mb-1">รูปโปรไฟล์ (URL)</label>
            <input type="url" id="charImage" class="w-full p-2.5 rounded-xl border border-warmBorder text-sm" placeholder="https://example.com/photo.jpg">
          </div>
          <div>
            <label class="block text-xs font-semibold text-warmMuted uppercase mb-1">บทบาท / อาชีพ</label>
            <input type="text" id="charRole" class="w-full p-2.5 rounded-xl border border-warmBorder text-sm" placeholder="เช่น นักพานิชย์, บาริสต้า">
          </div>
        </div>

        <div>
          <label class="block text-xs font-semibold text-warmMuted uppercase mb-1">ชีวประวัติ / ประวัติทั่วไป</label>
          <textarea id="charBio" rows="3" class="w-full p-2.5 rounded-xl border border-warmBorder text-sm" placeholder="เล่าเรื่องราวความเป็นมาของตัวละคร..."></textarea>
        </div>

        <div>
          <label class="block text-xs font-semibold text-warmMuted uppercase mb-1">อุปนิสัย / บุคลิกภาพ</label>
          <input type="text" id="charPersonality" class="w-full p-2.5 rounded-xl border border-warmBorder text-sm" placeholder="เช่น ใจดี ร่าเริง สุขุม">
        </div>

        <!-- Dynamic Custom Fields -->
        <div>
          <div class="flex justify-between items-center mb-2">
            <label class="block text-xs font-semibold text-warmMuted uppercase">ข้อมูลเพิ่มเติมที่กำหนดเอง (Dynamic Fields)</label>
            <button type="button" onclick="addCustomFieldRow()" class="text-xs text-warmAccent hover:underline">+ เพิ่มหัวข้อ</button>
          </div>
          <div id="customFieldsContainer" class="space-y-2"></div>
        </div>

        <div class="pt-4 border-t border-warmBorder flex justify-end space-x-2">
          <button type="button" onclick="closeModal('charModal')" class="px-4 py-2 text-warmMuted hover:bg-warmBg rounded-xl text-sm font-medium">ยกเลิก</button>
          <button type="submit" class="px-5 py-2 bg-warmPrimary text-white rounded-xl text-sm font-medium hover:bg-warmPrimaryHover">บันทึกข้อมูล</button>
        </div>
      </form>
    </div>
  </div>

  <!-- Modal: Add World -->
  <div id="worldModal" class="fixed inset-0 bg-black/40 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
    <div class="bg-white max-w-md w-full rounded-2xl p-6 border border-warmBorder shadow-xl relative">
      <button onclick="closeModal('worldModal')" class="absolute top-4 right-4 p-2 text-warmMuted hover:text-warmPrimary">
        <i data-lucide="x" class="w-5 h-5"></i>
      </button>
      <h3 class="heading text-xl font-semibold text-warmPrimary mb-4">เพิ่มโลก / จักรวาลใหม่</h3>
      <form id="worldForm" class="space-y-4">
        <div>
          <label class="block text-xs font-semibold text-warmMuted uppercase mb-1">ชื่อโลก / จักรวาล</label>
          <input type="text" id="worldName" required class="w-full p-2.5 rounded-xl border border-warmBorder text-sm" placeholder="เช่น เมืองท่าเอเดน, สถาบันเวทมนตร์">
        </div>
        <div>
          <label class="block text-xs font-semibold text-warmMuted uppercase mb-1">คำอธิบาย / Lore</label>
          <textarea id="worldDesc" rows="3" class="w-full p-2.5 rounded-xl border border-warmBorder text-sm" placeholder="รายละเอียดของโลกยุคสมัย กฎเกณฑ์..."></textarea>
        </div>
        <div class="pt-3 border-t border-warmBorder flex justify-end space-x-2">
          <button type="button" onclick="closeModal('worldModal')" class="px-4 py-2 text-warmMuted hover:bg-warmBg rounded-xl text-sm font-medium">ยกเลิก</button>
          <button type="submit" class="px-5 py-2 bg-warmPrimary text-white rounded-xl text-sm font-medium hover:bg-warmPrimaryHover">สร้างโลกใหม่</button>
        </div>
      </form>
    </div>
  </div>

  <!-- Modal: Add Relationship -->
  <div id="relModal" class="fixed inset-0 bg-black/40 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
    <div class="bg-white max-w-md w-full rounded-2xl p-6 border border-warmBorder shadow-xl relative">
      <button onclick="closeModal('relModal')" class="absolute top-4 right-4 p-2 text-warmMuted hover:text-warmPrimary">
        <i data-lucide="x" class="w-5 h-5"></i>
      </button>
      <h3 class="heading text-xl font-semibold text-warmPrimary mb-4">เชื่อมโยงความสัมพันธ์</h3>
      <form id="relForm" class="space-y-4">
        <div>
          <label class="block text-xs font-semibold text-warmMuted uppercase mb-1">ตัวละครต้นทาง</label>
          <select id="relFromChar" class="w-full p-2.5 rounded-xl border border-warmBorder text-sm" required></select>
        </div>
        <div>
          <label class="block text-xs font-semibold text-warmMuted uppercase mb-1">ตัวละครปลายทาง</label>
          <select id="relToChar" class="w-full p-2.5 rounded-xl border border-warmBorder text-sm" required></select>
        </div>
        <div>
          <label class="block text-xs font-semibold text-warmMuted uppercase mb-1">สถานะความสัมพันธ์</label>
          <input type="text" id="relType" class="w-full p-2.5 rounded-xl border border-warmBorder text-sm" placeholder="เช่น เพื่อนสนิท, ศัตรูหัวใจ, พี่น้อง" required>
        </div>
        <div class="pt-3 border-t border-warmBorder flex justify-end space-x-2">
          <button type="button" onclick="closeModal('relModal')" class="px-4 py-2 text-warmMuted hover:bg-warmBg rounded-xl text-sm font-medium">ยกเลิก</button>
          <button type="submit" class="px-5 py-2 bg-warmPrimary text-white rounded-xl text-sm font-medium hover:bg-warmPrimaryHover">เพิ่มเส้นเชื่อม</button>
        </div>
      </form>
    </div>
  </div>

  <!-- Firebase JavaScript Modules -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
    import { 
      getAuth, 
      createUserWithEmailAndPassword, 
      signInWithEmailAndPassword, 
      signOut, 
      onAuthStateChanged 
    } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
    import { 
      getFirestore, 
      collection, 
      doc,
      setDoc,
      addDoc, 
      getDocs, 
      deleteDoc,
      query, 
      where 
    } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

    // 🔑 Firebase Config ของคุณ
    const firebaseConfig = {
      apiKey: "AIzaSyANIvH48vw2KafDKyDi8MWgEKQfH7uAV-Q",
      authDomain: "oc-profile-eeb9a.firebaseapp.com",
      projectId: "oc-profile-eeb9a",
      storageBucket: "oc-profile-eeb9a.firebasestorage.app",
      messagingSenderId: "445589847402",
      appId: "1:445589847402:web:b8d57e6024bf3afd9bb913",
      measurementId: "G-4RZL978T5H"
    };

    // Initialize Firebase Services
    const app = initializeApp(firebaseConfig);
    const auth = getAuth(app);
    const db = getFirestore(app);

    // Global App States
    window.currentUser = null;
    window.characters = [];
    window.worlds = [];
    window.relationships = [];
    window.nodePositions = {};
    let isSignUpMode = false;

    // Initialize Icons
    lucide.createIcons();

    // DOM Elements
    const authScreen = document.getElementById('authScreen');
    const appDashboard = document.getElementById('appDashboard');
    const authHeaderState = document.getElementById('authHeaderState');
    const authForm = document.getElementById('authForm');
    const authEmail = document.getElementById('authEmail');
    const authPassword = document.getElementById('authPassword');
    const authTitle = document.getElementById('authTitle');
    const authSubmitBtn = document.getElementById('authSubmitBtn');
    const toggleAuthModeBtn = document.getElementById('toggleAuthModeBtn');
    const authError = document.getElementById('authError');

    // Toggle Sign In / Sign Up Mode
    toggleAuthModeBtn.addEventListener('click', () => {
      isSignUpMode = !isSignUpMode;
      authError.classList.add('hidden');
      if (isSignUpMode) {
        authTitle.innerText = "สมัครสมาชิกใหม่";
        authSubmitBtn.innerText = "ลงทะเบียน";
        toggleAuthModeBtn.innerText = "มีบัญชีอยู่แล้ว? เข้าสู่ระบบ";
      } else {
        authTitle.innerText = "เข้าสู่ระบบ \"ข้อมูล OC\"";
        authSubmitBtn.innerText = "เข้าสู่ระบบ";
        toggleAuthModeBtn.innerText = "ยังไม่มีบัญชี? สมัครสมาชิกใหม่ที่นี่";
      }
    });

    // Handle Form Auth Submission
    authForm.addEventListener('submit', async (e) => {
      e.preventDefault();
      authError.classList.add('hidden');
      const email = authEmail.value;
      const password = authPassword.value;

      try {
        if (isSignUpMode) {
          await createUserWithEmailAndPassword(auth, email, password);
        } else {
          await signInWithEmailAndPassword(auth, email, password);
        }
      } catch (err) {
        authError.classList.remove('hidden');
        if (err.code === 'auth/weak-password') {
          authError.innerText = "รหัสผ่านต้องมีความยาวอย่างน้อย 6 ตัวอักษร";
        } else if (err.code === 'auth/email-already-in-use') {
          authError.innerText = "อีเมลนี้ถูกใช้งานแล้ว โปรดกดเข้าสู่ระบบ";
        } else if (err.code === 'auth/invalid-credential' || err.code === 'auth/user-not-found' || err.code === 'auth/wrong-password') {
          authError.innerText = "อีเมลหรือรหัสผ่านไม่ถูกต้อง";
        } else {
          authError.innerText = "เกิดข้อผิดพลาด: " + err.message;
        }
      }
    });

    // Auth State Observer
    onAuthStateChanged(auth, async (user) => {
      window.currentUser = user;
      if (user) {
        authScreen.classList.add('hidden');
        appDashboard.classList.remove('hidden');
        authHeaderState.innerHTML = `
          <div class="flex items-center space-x-2 bg-warmBg px-3 py-1.5 rounded-xl border border-warmBorder">
            <i data-lucide="user" class="w-4 h-4 text-warmPrimary"></i>
            <span class="text-xs font-medium text-warmPrimary hidden sm:inline">${user.email}</span>
          </div>
          <button id="logoutBtn" class="px-3 py-1.5 border border-warmBorder hover:bg-red-50 text-red-600 rounded-xl text-xs font-medium transition-colors">
            ออกจากระบบ
          </button>
        `;
        document.getElementById('logoutBtn').addEventListener('click', () => signOut(auth));
        lucide.createIcons();

        // Load Firestore Data for this User
        await loadUserData();
      } else {
        authScreen.classList.remove('hidden');
        appDashboard.classList.add('hidden');
        authHeaderState.innerHTML = `<span class="text-xs text-warmMuted">ยังไม่ได้เข้าสู่ระบบ</span>`;
      }
    });

    // Load User Isolated Data from Firestore
    async function loadUserData() {
      if (!window.currentUser) return;
      const uid = window.currentUser.uid;

      // Fetch Worlds
      const worldsSnap = await getDocs(collection(db, `users/${uid}/worlds`));
      window.worlds = worldsSnap.docs.map(doc => ({ id: doc.id, ...doc.data() }));

      // Fetch Characters
      const charsSnap = await getDocs(collection(db, `users/${uid}/characters`));
      window.characters = charsSnap.docs.map(doc => ({ id: doc.id, ...doc.data() }));

      // Fetch Relationships
      const relsSnap = await getDocs(collection(db, `users/${uid}/relationships`));
      window.relationships = relsSnap.docs.map(doc => ({ id: doc.id, ...doc.data() }));

      // Populate Dropdowns & Views
      updateWorldDropdowns();
      renderCharacters();
      renderWorlds();
      renderCompareOptions();
    }

    // Update Dropdown Selection lists
    function updateWorldDropdowns() {
      const filterSelect = document.getElementById('worldFilterSelect');
      const charWorldSelect = document.getElementById('charWorldId');

      filterSelect.innerHTML = '<option value="ALL">ทั้งหมด (ทุกจักรวาล)</option>';
      charWorldSelect.innerHTML = '<option value="">-- ไม่ระบุโลก --</option>';

      window.worlds.forEach(w => {
        filterSelect.innerHTML += `<option value="${w.id}">${w.name}</option>`;
        charWorldSelect.innerHTML += `<option value="${w.id}">${w.name}</option>`;
      });
    }

    // Save Handlers to Firestore
    document.getElementById('worldForm').addEventListener('submit', async (e) => {
      e.preventDefault();
      if (!window.currentUser) return;
      const name = document.getElementById('worldName').value;
      const desc = document.getElementById('worldDesc').value;

      const docRef = await addDoc(collection(db, `users/${window.currentUser.uid}/worlds`), {
        name, desc, createdAt: new Date()
      });

      window.worlds.push({ id: docRef.id, name, desc });
      updateWorldDropdowns();
      renderWorlds();
      closeModal('worldModal');
      document.getElementById('worldForm').reset();
    });

    document.getElementById('charForm').addEventListener('submit', async (e) => {
      e.preventDefault();
      if (!window.currentUser) return;

      const id = document.getElementById('charId').value;
      const name = document.getElementById('charName').value;
      const worldId = document.getElementById('charWorldId').value;
      const image = document.getElementById('charImage').value || 'https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=300&q=80';
      const role = document.getElementById('charRole').value;
      const bio = document.getElementById('charBio').value;
      const personality = document.getElementById('charPersonality').value;

      // Collect custom dynamic fields
      const customFieldRows = document.querySelectorAll('.custom-field-row');
      const customFields = [];
      customFieldRows.forEach(row => {
        const label = row.querySelector('.field-label').value;
        const val = row.querySelector('.field-value').value;
        if (label) customFields.push({ label, val });
      });

      const charData = { name, worldId, image, role, bio, personality, customFields, updatedAt: new Date() };

      if (id) {
        await setDoc(doc(db, `users/${window.currentUser.uid}/characters`, id), charData, { merge: true });
        const idx = window.characters.findIndex(c => c.id === id);
        if (idx !== -1) window.characters[idx] = { id, ...charData };
      } else {
        const docRef = await addDoc(collection(db, `users/${window.currentUser.uid}/characters`), charData);
        window.characters.push({ id: docRef.id, ...charData });
      }

      renderCharacters();
      renderCompareOptions();
      closeModal('charModal');
      document.getElementById('charForm').reset();
    });

    document.getElementById('relForm').addEventListener('submit', async (e) => {
      e.preventDefault();
      if (!window.currentUser) return;

      const from = document.getElementById('relFromChar').value;
      const to = document.getElementById('relToChar').value;
      const type = document.getElementById('relType').value;

      if (from === to) {
        alert('ไม่สามารถเชื่อมโยงตัวละครเดียวกันได้');
        return;
      }

      const docRef = await addDoc(collection(db, `users/${window.currentUser.uid}/relationships`), {
        from, to, type
      });

      window.relationships.push({ id: docRef.id, from, to, type });
      renderCanvasNodes();
      closeModal('relModal');
      document.getElementById('relForm').reset();
    });

    // Make Functions Globally Accessible
    window.renderCharacters = function() {
      const grid = document.getElementById('charGrid');
      const filter = document.getElementById('worldFilterSelect').value;
      
      const filtered = filter === 'ALL' 
        ? window.characters 
        : window.characters.filter(c => c.worldId === filter);

      document.getElementById('charCountLabel').innerText = `พบทั้งหมด ${filtered.length} ตัวละคร`;

      if (filtered.length === 0) {
        grid.innerHTML = `
          <div class="col-span-full text-center py-12 bg-white rounded-2xl border border-warmBorder">
            <i data-lucide="ghost" class="w-12 h-12 text-warmMuted mx-auto mb-2"></i>
            <p class="text-sm font-medium text-warmMuted">ยังไม่มีข้อมูล OC ในหมวดนี้</p>
            <button onclick="openCharModal()" class="mt-3 text-xs text-warmAccent hover:underline font-semibold">+ เพิ่ม OC ตัวแรกของคุณ</button>
          </div>
        `;
        lucide.createIcons();
        return;
      }

      grid.innerHTML = filtered.map(c => {
        const world = window.worlds.find(w => w.id === c.worldId);
        return `
          <div class="bg-white rounded-2xl border border-warmBorder overflow-hidden shadow-sm hover:shadow-md transition-all flex flex-col">
            <div class="h-48 w-full bg-warmBg relative overflow-hidden">
              <img src="${c.image}" class="w-full h-full object-cover object-top" onerror="this.src='https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=300&q=80'">
              ${world ? `<span class="absolute top-2 right-2 bg-white/90 backdrop-blur-md px-2.5 py-1 rounded-lg text-[10px] font-semibold text-warmPrimary border border-warmBorder">${world.name}</span>` : ''}
            </div>
            <div class="p-4 flex-1 flex flex-col justify-between">
              <div>
                <h4 class="heading font-semibold text-base text-warmPrimary">${c.name}</h4>
                <p class="text-xs text-warmMuted mb-2">${c.role || 'ไม่ระบุบทบาท'}</p>
                <p class="text-xs text-warmMuted line-clamp-2">${c.bio || 'ไม่มีคำอธิบายประวัติ'}</p>
              </div>
              <div class="mt-4 pt-3 border-t border-warmBorder flex items-center justify-between">
                <button onclick="viewCharProfile('${c.id}')" class="text-xs font-semibold text-warmAccent hover:underline flex items-center">
                  <i data-lucide="eye" class="w-3.5 h-3.5 mr-1"></i> ดูโปรไฟล์เต็ม
                </button>
                <button onclick="deleteChar('${c.id}')" class="text-warmMuted hover:text-red-500 p-1">
                  <i data-lucide="trash-2" class="w-4 h-4"></i>
                </button>
              </div>
            </div>
          </div>
        `;
      }).join('');

      lucide.createIcons();
    };

    window.renderWorlds = function() {
      const grid = document.getElementById('worldGrid');
      if (window.worlds.length === 0) {
        grid.innerHTML = `<div class="col-span-full text-center py-8 text-warmMuted text-sm">ยังไม่ได้สร้างโลก / จักรวาล</div>`;
        return;
      }

      grid.innerHTML = window.worlds.map(w => {
        const count = window.characters.filter(c => c.worldId === w.id).length;
        return `
          <div class="bg-white p-5 rounded-2xl border border-warmBorder shadow-sm">
            <div class="flex justify-between items-start mb-2">
              <h4 class="heading font-semibold text-base text-warmPrimary">${w.name}</h4>
              <span class="text-xs px-2.5 py-0.5 bg-amber-50 text-warmAccent font-semibold rounded-full border border-amber-200">${count} OC</span>
            </div>
            <p class="text-xs text-warmMuted mb-4">${w.desc || 'ไม่มีคำอธิบาย Lore'}</p>
            <button onclick="filterByWorld('${w.id}')" class="text-xs text-warmPrimary font-semibold hover:underline flex items-center">
              ดูสมาชิกในโลกนี้ <i data-lucide="arrow-right" class="w-3.5 h-3.5 ml-1"></i>
            </button>
          </div>
        `;
      }).join('');
      lucide.createIcons();
    };

    window.viewCharProfile = function(id) {
      const c = window.characters.find(char => char.id === id);
      if (!c) return;
      const world = window.worlds.find(w => w.id === c.worldId);

      let customFieldsHtml = '';
      if (c.customFields && c.customFields.length > 0) {
        customFieldsHtml = `
          <div class="mt-4 pt-4 border-t border-warmBorder">
            <h5 class="text-xs font-semibold text-warmMuted uppercase mb-2">ข้อมูลเพิ่มเติม</h5>
            <div class="grid grid-cols-2 gap-2">
              ${c.customFields.map(f => `
                <div class="bg-warmBg/60 p-2.5 rounded-xl border border-warmBorder">
                  <span class="block text-[10px] text-warmMuted font-semibold uppercase">${f.label}</span>
                  <span class="text-xs font-medium text-warmPrimary">${f.val}</span>
                </div>
              `).join('')}
            </div>
          </div>
        `;
      }

      document.getElementById('viewCharContent').innerHTML = `
        <div class="flex flex-col md:flex-row gap-6">
          <div class="w-full md:w-1/3">
            <img src="${c.image}" class="w-full h-64 object-cover rounded-2xl border border-warmBorder shadow-sm">
          </div>
          <div class="w-full md:w-2/3 space-y-3">
            <div>
              <div class="flex justify-between items-start">
                <h3 class="heading text-2xl font-bold text-warmPrimary">${c.name}</h3>
                <button onclick="openCharModal('${c.id}')" class="px-3 py-1 bg-warmBg border border-warmBorder hover:bg-white text-xs font-semibold rounded-lg">แก้ไข</button>
              </div>
              <p class="text-sm font-medium text-warmAccent">${c.role || 'ไม่ระบุบทบาท'} ${world ? `• โลก ${world.name}` : ''}</p>
            </div>
            
            <div class="bg-warmBg p-3 rounded-xl border border-warmBorder">
              <span class="block text-[10px] font-semibold text-warmMuted uppercase">อุปนิสัย</span>
              <p class="text-xs text-warmPrimary font-medium">${c.personality || 'ไม่ระบุ'}</p>
            </div>

            <div>
              <span class="block text-[10px] font-semibold text-warmMuted uppercase mb-1">ชีวประวัติทั่วไป</span>
              <p class="text-xs text-warmMuted leading-relaxed whitespace-pre-line">${c.bio || 'ไม่มีข้อมูลประวัติ'}</p>
            </div>

            ${customFieldsHtml}
          </div>
        </div>
      `;

      document.getElementById('viewCharModal').classList.remove('hidden');
    };

    window.deleteChar = async function(id) {
      if (!confirm('ยืนยันลบตัวละครนี้ใช่หรือไม่?')) return;
      await deleteDoc(doc(db, `users/${window.currentUser.uid}/characters`, id));
      window.characters = window.characters.filter(c => c.id !== id);
      renderCharacters();
    };

    window.switchTab = function(tab) {
      const tabs = ['chars', 'worlds', 'rel', 'compare'];
      tabs.forEach(t => {
        document.getElementById(`${t}Section`).classList.add('hidden');
        document.getElementById(`tab${t.charAt(0).toUpperCase() + t.slice(1)}Btn`).className = 
          'px-4 py-2 rounded-xl text-sm font-medium text-warmMuted hover:bg-warmBg whitespace-nowrap';
      });

      document.getElementById(`${tab}Section`).classList.remove('hidden');
      document.getElementById(`tab${tab.charAt(0).toUpperCase() + tab.slice(1)}Btn`).className = 
        'px-4 py-2 rounded-xl text-sm font-medium bg-warmPrimary text-white shadow-sm whitespace-nowrap';

      document.getElementById('charFilterBar').style.display = (tab === 'chars') ? 'flex' : 'none';

      if (tab === 'rel') renderCanvasNodes();
      if (tab === 'compare') renderCompare();
    };

    window.openCharModal = function(id = null) {
      const modal = document.getElementById('charModal');
      const form = document.getElementById('charForm');
      form.reset();
      document.getElementById('customFieldsContainer').innerHTML = '';

      if (id) {
        const c = window.characters.find(item => item.id === id);
        if (c) {
          document.getElementById('charModalTitle').innerText = 'แก้ไขข้อมูล OC';
          document.getElementById('charId').value = c.id;
          document.getElementById('charName').value = c.name;
          document.getElementById('charWorldId').value = c.worldId || '';
          document.getElementById('charImage').value = c.image;
          document.getElementById('charRole').value = c.role || '';
          document.getElementById('charBio').value = c.bio || '';
          document.getElementById('charPersonality').value = c.personality || '';

          if (c.customFields) {
            c.customFields.forEach(f => addCustomFieldRow(f.label, f.val));
          }
        }
      } else {
        document.getElementById('charModalTitle').innerText = 'เพิ่มข้อมูล OC ใหม่';
        document.getElementById('charId').value = '';
      }

      closeModal('viewCharModal');
      modal.classList.remove('hidden');
    };

    window.openWorldModal = () => document.getElementById('worldModal').classList.remove('hidden');
    
    window.openAddRelModal = function() {
      const fromSelect = document.getElementById('relFromChar');
      const toSelect = document.getElementById('relToChar');
      
      fromSelect.innerHTML = window.characters.map(c => `<option value="${c.id}">${c.name}</option>`).join('');
      toSelect.innerHTML = window.characters.map(c => `<option value="${c.id}">${c.name}</option>`).join('');
      
      document.getElementById('relModal').classList.remove('hidden');
    };

    window.closeModal = id => document.getElementById(id).classList.add('hidden');

    window.addCustomFieldRow = function(label = '', val = '') {
      const container = document.getElementById('customFieldsContainer');
      const div = document.createElement('div');
      div.className = 'flex items-center space-x-2 custom-field-row';
      div.innerHTML = `
        <input type="text" class="field-label w-1/3 p-2 rounded-xl border border-warmBorder text-xs" placeholder="หัวข้อ (เช่น วันเกิด, ของชอบ)" value="${label}">
        <input type="text" class="field-value w-2/3 p-2 rounded-xl border border-warmBorder text-xs" placeholder="รายละเอียด" value="${val}">
        <button type="button" onclick="this.parentElement.remove()" class="text-red-400 hover:text-red-600 p-1"><i data-lucide="x" class="w-4 h-4"></i></button>
      `;
      container.appendChild(div);
      lucide.createIcons();
    };

    window.filterByWorld = function(worldId) {
      document.getElementById('worldFilterSelect').value = worldId;
      switchTab('chars');
      renderCharacters();
    };

    // Render Canvas Interactive Graph
    function renderCanvasNodes() {
      const container = document.getElementById('nodesContainer');
      const svg = document.getElementById('relSvg');
      container.innerHTML = '';
      svg.innerHTML = '';

      if (window.characters.length === 0) return;

      const width = container.clientWidth || 600;
      const height = container.clientHeight || 500;
      const radius = Math.min(width, height) / 3;
      const centerX = width / 2;
      const centerY = height / 2;

      // Position nodes in circle if not moved
      window.characters.forEach((c, idx) => {
        if (!window.nodePositions[c.id]) {
          const angle = (idx / window.characters.length) * 2 * Math.PI;
          window.nodePositions[c.id] = {
            x: centerX + radius * Math.cos(angle) - 30,
            y: centerY + radius * Math.sin(angle) - 30
          };
        }
      });

      // Draw SVG lines
      window.relationships.forEach(rel => {
        const posA = window.nodePositions[rel.from];
        const posB = window.nodePositions[rel.to];
        if (posA && posB) {
          const x1 = posA.x + 30;
          const y1 = posA.y + 30;
          const x2 = posB.x + 30;
          const y2 = posB.y + 30;

          svg.innerHTML += `
            <line x1="${x1}" y1="${y1}" x2="${x2}" y2="${y2}" stroke="#D97706" stroke-width="2" stroke-dasharray="4" />
            <text x="${(x1 + x2) / 2}" y="${(y1 + y2) / 2}" fill="#7C6A58" font-size="11" font-weight="bold" text-anchor="middle" background="white" class="bg-white">${rel.type}</text>
          `;
        }
      });

      // Draw Nodes
      window.characters.forEach(c => {
        const pos = window.nodePositions[c.id];
        const node = document.createElement('div');
        node.className = 'absolute w-14 h-14 rounded-full border-2 border-warmAccent bg-white shadow-md cursor-grab flex items-center justify-center select-none';
        node.style.left = `${pos.x}px`;
        node.style.top = `${pos.y}px`;
        node.innerHTML = `
          <img src="${c.image}" class="w-full h-full object-cover rounded-full pointer-events-none">
          <span class="absolute -bottom-5 text-[10px] font-bold text-warmPrimary whitespace-nowrap bg-white/80 px-1.5 rounded border border-warmBorder pointer-events-none">${c.name}</span>
        `;

        // Touch / Mouse Dragging
        let isDragging = false;
        let offsetX, offsetY;

        const onStart = (e) => {
          isDragging = true;
          const clientX = e.touches ? e.touches[0].clientX : e.clientX;
          const clientY = e.touches ? e.touches[0].clientY : e.clientY;
          offsetX = clientX - pos.x;
          offsetY = clientY - pos.y;
        };

        const onMove = (e) => {
          if (!isDragging) return;
          const clientX = e.touches ? e.touches[0].clientX : e.clientX;
          const clientY = e.touches ? e.touches[0].clientY : e.clientY;
          pos.x = clientX - offsetX;
          pos.y = clientY - offsetY;
          node.style.left = `${pos.x}px`;
          node.style.top = `${pos.y}px`;
          renderCanvasNodes();
        };

        const onEnd = () => { isDragging = false; };

        node.addEventListener('mousedown', onStart);
        window.addEventListener('mousemove', onMove);
        window.addEventListener('mouseup', onEnd);

        node.addEventListener('touchstart', onStart);
        window.addEventListener('touchmove', onMove);
        window.addEventListener('touchend', onEnd);

        container.appendChild(node);
      });
    }

    // Compare OC Feature
    function renderCompareOptions() {
      const select1 = document.getElementById('compareChar1');
      const select2 = document.getElementById('compareChar2');
      if (!select1 || !select2) return;

      select1.innerHTML = window.characters.map(c => `<option value="${c.id}">${c.name}</option>`).join('');
      select2.innerHTML = window.characters.map(c => `<option value="${c.id}">${c.name}</option>`).join('');

      if (window.characters.length > 1) select2.selectedIndex = 1;
      renderCompare();
    }

    window.renderCompare = function() {
      const id1 = document.getElementById('compareChar1').value;
      const id2 = document.getElementById('compareChar2').value;
      const res = document.getElementById('compareResult');

      const c1 = window.characters.find(c => c.id === id1);
      const c2 = window.characters.find(c => c.id === id2);

      if (!c1 || !c2) {
        res.innerHTML = '<div class="col-span-2 text-center py-8 text-warmMuted text-sm">ต้องมีตัวละครอย่างน้อย 2 ตัวในการเปรียบเทียบ</div>';
        return;
      }

      res.innerHTML = [c1, c2].map(c => `
        <div class="bg-white p-5 rounded-2xl border border-warmBorder shadow-sm space-y-3">
          <div class="flex items-center space-x-3">
            <img src="${c.image}" class="w-16 h-16 rounded-xl object-cover border border-warmBorder">
            <div>
              <h4 class="heading text-lg font-semibold text-warmPrimary">${c.name}</h4>
              <p class="text-xs text-warmAccent font-medium">${c.role || 'ไม่ระบุบทบาท'}</p>
            </div>
          </div>
          <div class="bg-warmBg p-3 rounded-xl text-xs space-y-1">
            <span class="font-semibold text-warmMuted uppercase block">อุปนิสัย</span>
            <p class="text-warmPrimary font-medium">${c.personality || '-'}</p>
          </div>
          <div class="text-xs text-warmMuted">
            <span class="font-semibold uppercase block mb-1">ประวัติ</span>
            <p class="line-clamp-4">${c.bio || '-'}</p>
          </div>
        </div>
      `).join('');
    };
  </script>
</body>
</html>
