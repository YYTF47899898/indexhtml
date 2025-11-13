<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Library Management System (Light Blue Theme)</title>
  <style>
    :root{
      /* Light Blue Theme */
      --bg: #f3f8ff;
      --card: #ffffff;
      --muted: #6b7280;
      --accent: #1e90ff;    /* primary blue */
      --accent-600: #187bde;
      --accent-100: #dceeff;
      --success: #16a34a;
      --danger: #ef4444;
      --radius: 10px;
      --pad: 14px;
      --maxw: 1200px;
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    }
    *{box-sizing:border-box}
    body{margin:0;background:var(--bg);color:#0f172a}
    .wrap{max-width:var(--maxw);margin:28px auto;padding:20px}
    header{
      display:flex;align-items:center;justify-content:space-between;margin-bottom:14px;
      background: linear-gradient(90deg, rgba(30,144,255,0.08), rgba(30,144,255,0.03));
      padding:18px;border-radius:12px;
      box-shadow: 0 8px 30px rgba(30,144,255,0.05);
    }
    h1{font-size:24px;margin:0;color:#032b57}
    nav{display:flex;gap:8px}
    .btn{padding:8px 12px;border-radius:8px;border:1px solid transparent;background:transparent;cursor:pointer;font-size:14px;color:#074a9a}
    .btn:hover{background:var(--accent-100)}
    .btn.active{background:var(--accent);color:#fff;border-color:var(--accent-600);box-shadow:0 6px 18px rgba(30,144,255,0.12)}
    .grid{display:grid;gap:14px}
    .grid.cols-3{grid-template-columns:repeat(3,1fr)}
    .card{background:var(--card);padding:var(--pad);border-radius:var(--radius);box-shadow:0 6px 18px rgba(8,24,48,0.04)}
    .card .title{color:var(--muted);font-size:13px}
    .card .num{font-size:22px;font-weight:700;margin-top:6px;color:#042f6b}
    .layout{display:flex;gap:14px;margin-top:18px}
    .left{flex:1}
    .right{width:380px}
    .panel{background:var(--card);padding:var(--pad);border-radius:var(--radius);box-shadow:0 6px 18px rgba(8,24,48,0.04)}
    .controls{display:flex;gap:8px;align-items:center}
    input[type="text"], input[type="email"], input[type="tel"], select, input[type="number"]{
      padding:10px 12px;border-radius:10px;border:1px solid #e6eefb;font-size:14px;width:100%;
      background: linear-gradient(180deg, #fff, #fbfdff);
    }
    table{width:100%;border-collapse:collapse;background:transparent}
    th, td{padding:12px 10px;text-align:left;border-bottom:1px solid #f1f5fb;font-size:14px}
    th{background:linear-gradient(180deg,#fbfdff,#f8fbff);color:#0b3d73}
    .actions a{margin-right:10px;color:var(--accent);text-decoration:none;font-size:13px;cursor:pointer}
    .muted{color:var(--muted);font-size:13px}
    .footer{margin-top:22px;text-align:center;color:var(--muted);font-size:13px}
    .notify{position:fixed;right:18px;bottom:18px;background:var(--card);padding:10px 14px;border-radius:10px;box-shadow:0 10px 30px rgba(8,24,48,0.08);border-left:4px solid var(--accent)}
    .form-row{display:grid;grid-template-columns:repeat(6,1fr);gap:8px}
    .form-row .col-3{grid-column:span 3}
    .form-row .col-2{grid-column:span 2}
    .form-row .col-1{grid-column:span 1}
    .form-actions{display:flex;gap:8px;margin-top:12px}
    .btn-primary{background:var(--accent);color:white;border:none;padding:9px 14px;border-radius:8px}
    .btn-outline{background:transparent;border:1px solid #e6eefb;padding:8px 12px;border-radius:8px}
    .accent-pill{display:inline-block;padding:6px 10px;border-radius:999px;background:var(--accent-100);color:var(--accent);font-weight:600}
    .small{font-size:13px}
    @media (max-width:980px){
      .layout{flex-direction:column}
      .right{width:100%}
      .grid.cols-3{grid-template-columns:1fr}
      .form-row{grid-template-columns:repeat(2,1fr)}
      .form-row .col-3{grid-column:span 2}
      .form-row .col-2{grid-column:span 2}
    }
    /* highlight available low stock */
    .low-stock td:nth-child(5){color:var(--danger);font-weight:600}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <h1>LIBRARY MANAGEMENT SYSTEM</h1>
      <nav>
        <button class="btn active" id="nav-dashboard">Dashboard</button>
        <button class="btn" id="nav-books">Books</button>
        <button class="btn" id="nav-members">Members</button>
        <button class="btn" id="nav-issue">Issue / Return</button>
      </nav>
    </header>

    <div id="notification-placeholder"></div>

    <!-- Dashboard -->
    <div id="view-dashboard" class="view">
      <section class="grid cols-3" style="margin-bottom:16px">
        <div class="card">
          <div class="title">Books</div>
          <div class="num" id="stat-books">0</div>
          <div class="muted">Total titles stored</div>
        </div>
        <div class="card">
          <div class="title">Members</div>
          <div class="num" id="stat-members">0</div>
          <div class="muted">Registered library members</div>
        </div>
        <div class="card">
          <div class="title">Currently Issued</div>
          <div class="num" id="stat-issued">0</div>
          <div class="muted">Active issued copies</div>
        </div>
      </section>
    </div>

    <!-- Books view -->
    <div id="view-books" class="view" style="display:none">
      <div class="panel" style="margin-bottom:12px">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
          <h2 style="margin:0">Books</h2>
          <div style="display:flex;gap:8px;align-items:center">
            <input type="text" id="search-books" placeholder="Search books..." />
            <button class="btn-outline" id="export-books">Export CSV</button>
            <button class="btn-primary" id="btn-new-book">+ New</button>
          </div>
        </div>

        <form id="book-form">
          <div class="form-row" style="align-items:center">
            <input class="col-3" type="text" placeholder="Title" id="book-title" required />
            <input class="col-2" type="text" placeholder="Author" id="book-author" />
            <input class="col-1" type="text" placeholder="ISBN" id="book-isbn" />
            <input class="col-1" type="number" min="1" placeholder="Copies" id="book-copies" value="1" />
            <input class="col-1" type="text" placeholder="Category" id="book-category" />
            <input type="hidden" id="book-id" />
          </div>
          <div class="form-actions">
            <button class="btn-primary" type="submit">Save</button>
            <button type="button" class="btn-outline" id="book-clear">Clear</button>
          </div>
        </form>
      </div>

      <div class="panel">
        <div style="overflow-x:auto">
          <table id="books-table">
            <thead>
              <tr><th>ID</th><th>Title</th><th>Author</th><th>Copies</th><th>Available</th><th>Actions</th></tr>
            </thead>
            <tbody></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- Members view -->
    <div id="view-members" class="view" style="display:none">
      <div class="panel" style="margin-bottom:12px">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
          <h2 style="margin:0">Members</h2>
          <div style="display:flex;gap:8px;align-items:center">
            <button class="btn-outline" id="export-members">Export CSV</button>
            <button class="btn-primary" id="btn-new-member">+ New</button>
          </div>
        </div>

        <form id="member-form">
          <div class="form-row" style="align-items:center">
            <input class="col-3" type="text" placeholder="Name" id="member-name" required />
            <input class="col-2" type="email" placeholder="Email" id="member-email" />
            <input class="col-1" type="tel" placeholder="Phone" id="member-phone" />
            <input type="hidden" id="member-id" />
          </div>
          <div class="form-actions">
            <button class="btn-primary" type="submit">Save</button>
            <button type="button" class="btn-outline" id="member-clear">Clear</button>
          </div>
        </form>
      </div>

      <div class="panel">
        <div style="overflow-x:auto">
          <table id="members-table">
            <thead>
              <tr><th>ID</th><th>Name</th><th>Email</th><th>Phone</th><th>Actions</th></tr>
            </thead>
            <tbody></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- Issue view -->
    <div id="view-issue" class="view" style="display:none">
      <div class="layout">
        <div class="left">
          <div class="panel">
            <h3 style="margin-top:0">Issue / Return</h3>
            <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px">
              <div>
                <label class="muted">Select Book</label>
                <select id="issue-book"></select>
              </div>
              <div>
                <label class="muted">Select Member</label>
                <select id="issue-member"></select>
              </div>
              <div style="grid-column:span 2;display:flex;gap:8px;justify-content:flex-end">
                <button class="btn-primary" id="issue-14">Issue 14d</button>
                <button class="btn-outline" id="issue-7">Issue 7d</button>
              </div>
            </div>
          </div>

          <div style="height:12px"></div>

          <div class="panel">
            <h4 style="margin-top:0">Active Issues</h4>
            <div style="overflow-x:auto">
              <table id="issues-table">
                <thead><tr><th>Txn ID</th><th>Book</th><th>Member</th><th>Due</th><th>Actions</th></tr></thead>
                <tbody></tbody>
              </table>
            </div>
          </div>
        </div>

        <aside class="right">
          <div class="panel">
            <h4 style="margin-top:0">Quick Stats</h4>
            <div style="margin-top:8px"><strong id="quick-books">0</strong> Books</div>
            <div style="margin-top:6px"><strong id="quick-members">0</strong> Members</div>
            <div style="margin-top:6px"><strong id="quick-issued">0</strong> Issued</div>
          </div>
        </aside>
      </div>
    </div>

    <div class="footer">Made by <b>Rajat Singh</b>• Data stored securely</div>
  </div>

  <div id="toast" class="notify" style="display:none"></div>

  <script>
    /* ========== Storage keys & seed data (15 books) ========== */
    const KEY_BOOKS = 'lms_books';
    const KEY_MEMBERS = 'lms_members';
    const KEY_TX = 'lms_transactions';

    function seedBooks(){
      return [
        { id:'B_001', title:'Introduction to Algorithms', author:'Cormen, Leiserson et al.', isbn:'0262033844', copies:3, category:'Algorithms' },
        { id:'B_002', title:'Clean Code', author:'Robert C. Martin', isbn:'0132350882', copies:2, category:'Software Engineering' },
        { id:'B_003', title:'Discrete Mathematics', author:'Rosen', isbn:'0073383090', copies:2, category:'Mathematics' },
        { id:'B_004', title:'Operating System Concepts', author:'Silberschatz', isbn:'1118063333', copies:4, category:'OS' },
        { id:'B_005', title:'Computer Networks', author:'Tanenbaum', isbn:'0132126958', copies:3, category:'Networks' },
        { id:'B_006', title:'Database System Concepts', author:'Korth', isbn:'0073523321', copies:3, category:'Databases' },
        { id:'B_007', title:'Artificial Intelligence: A Modern Approach', author:'Russell & Norvig', isbn:'0136042597', copies:2, category:'AI' },
        { id:'B_008', title:'Design Patterns', author:'Gamma et al.', isbn:'0201633612', copies:2, category:'Software Design' },
        { id:'B_009', title:'Compilers: Principles, Techniques', author:'Aho et al.', isbn:'0321486811', copies:2, category:'Compilers' },
        { id:'B_010', title:'The Pragmatic Programmer', author:'Andrew Hunt', isbn:'020161622X', copies:2, category:'Software Engineering' },
        { id:'B_011', title:'Modern Operating Systems', author:'Andrew S. Tanenbaum', isbn:'013359162X', copies:3, category:'OS' },
        { id:'B_012', title:'Algorithms (Sedgewick)', author:'Sedgewick', isbn:'032157351X', copies:2, category:'Algorithms' },
        { id:'B_013', title:'Computer Organization and Design', author:'Patterson', isbn:'0124077269', copies:3, category:'Architecture' },
        { id:'B_014', title:'Introduction to Machine Learning', author:'Alpaydin', isbn:'0262012430', copies:2, category:'ML' },
        { id:'B_015', title:'Data Structures Using C', author:'Gosling', isbn:'0132313639', copies:4, category:'Data Structures' }
      ];
    }

    function seedMembers(){
      return [
        { id:'M_111AAAA', name:'Rajat Singh', email:'rajat@example.com', phone:'9999999999' },
        { id:'M_222BBBB', name:'Anita Sharma', email:'anita@example.com', phone:'8888888888' }
      ];
    }

    // read/write helpers
    function read(key, fallback){ try{ const r = localStorage.getItem(key); return r ? JSON.parse(r) : fallback; }catch(e){console.error(e); return fallback;} }
    function write(key, value){ try{ localStorage.setItem(key, JSON.stringify(value)); }catch(e){console.error(e);} }

    // initial state
    let books = read(KEY_BOOKS, seedBooks());
    let members = read(KEY_MEMBERS, seedMembers());
    let transactions = read(KEY_TX, []);

    // UI element refs
    const views = {
      dashboard: document.getElementById('view-dashboard'),
      books: document.getElementById('view-books'),
      members: document.getElementById('view-members'),
      issue: document.getElementById('view-issue')
    };
    const navButtons = {
      dashboard: document.getElementById('nav-dashboard'),
      books: document.getElementById('nav-books'),
      members: document.getElementById('nav-members'),
      issue: document.getElementById('nav-issue'),
    };

    const booksTableBody = document.querySelector('#books-table tbody');
    const membersTableBody = document.querySelector('#members-table tbody');
    const issuesTableBody = document.querySelector('#issues-table tbody');
    const searchBooksInput = document.getElementById('search-books');

    // book form elements
    const bookForm = document.getElementById('book-form');
    const bookTitle = document.getElementById('book-title');
    const bookAuthor = document.getElementById('book-author');
    const bookISBN = document.getElementById('book-isbn');
    const bookCopies = document.getElementById('book-copies');
    const bookCategory = document.getElementById('book-category');
    const bookIdInput = document.getElementById('book-id');
    const bookClearBtn = document.getElementById('book-clear');
    const btnNewBook = document.getElementById('btn-new-book');
    const exportBooksBtn = document.getElementById('export-books');

    // member form elements
    const memberForm = document.getElementById('member-form');
    const memberName = document.getElementById('member-name');
    const memberEmail = document.getElementById('member-email');
    const memberPhone = document.getElementById('member-phone');
    const memberIdInput = document.getElementById('member-id');
    const memberClearBtn = document.getElementById('member-clear');
    const btnNewMember = document.getElementById('btn-new-member');
    const exportMembersBtn = document.getElementById('export-members');

    // issue controls
    const issueBookSelect = document.getElementById('issue-book');
    const issueMemberSelect = document.getElementById('issue-member');
    const issue14Btn = document.getElementById('issue-14');
    const issue7Btn = document.getElementById('issue-7');

    // stats
    const statBooks = document.getElementById('stat-books');
    const statMembers = document.getElementById('stat-members');
    const statIssued = document.getElementById('stat-issued');
    const quickBooks = document.getElementById('quick-books');
    const quickMembers = document.getElementById('quick-members');
    const quickIssued = document.getElementById('quick-issued');

    // toast/notify
    const toast = document.getElementById('toast');
    let toastTimer = null;
    function notify(msg){
      toast.textContent = msg;
      toast.style.display = 'block';
      clearTimeout(toastTimer);
      toastTimer = setTimeout(()=> toast.style.display = 'none', 3000);
    }

    // helpers
    function genId(prefix='X'){ return prefix + '_' + Math.random().toString(36).slice(2,9).toUpperCase(); }
    function addDays(date, days){ const d = new Date(date); d.setDate(d.getDate() + days); return d; }

    function getAvailableCopies(bookId){
      const b = books.find(x => x.id === bookId);
      if(!b) return 0;
      const issued = transactions.filter(t => t.bookId === bookId && t.type === 'issue' && !t.returned).length;
      return Math.max(0, Number(b.copies||0) - issued);
    }

    // render functions
    function renderStats(){
      statBooks.textContent = books.length;
      statMembers.textContent = members.length;
      statIssued.textContent = transactions.filter(t=>t.type==='issue' && !t.returned).length;
      quickBooks.textContent = books.length;
      quickMembers.textContent = members.length;
      quickIssued.textContent = transactions.filter(t=>t.type==='issue' && !t.returned).length;
    }

    function renderBooks(filter=''){
      booksTableBody.innerHTML = '';
      const q = (filter||'').trim().toLowerCase();
      const list = books.filter(b => {
        if(!q) return true;
        return [b.title,b.author,b.isbn,b.category,b.id].some(f => (f||'').toString().toLowerCase().includes(q));
      });
      list.forEach(b => {
        const tr = document.createElement('tr');
        if(getAvailableCopies(b.id) === 0) tr.classList.add('low-stock');
        tr.innerHTML = `
          <td>${b.id}</td>
          <td>${escapeHtml(b.title)}</td>
          <td>${escapeHtml(b.author||'')}</td>
          <td>${b.copies}</td>
          <td>${getAvailableCopies(b.id)}</td>
          <td class="actions">
            <a data-action="edit" data-id="${b.id}">Edit</a>
            <a data-action="issue" data-id="${b.id}">Issue</a>
            <a data-action="delete" data-id="${b.id}" style="color:${'var(--danger)'}">Delete</a>
          </td>
        `;
        booksTableBody.appendChild(tr);
      });
      populateIssueBookSelect();
    }

    function renderMembers(){
      membersTableBody.innerHTML = '';
      members.forEach(m => {
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${m.id}</td>
          <td>${escapeHtml(m.name)}</td>
          <td>${escapeHtml(m.email||'')}</td>
          <td>${escapeHtml(m.phone||'')}</td>
          <td class="actions">
            <a data-action="edit" data-id="${m.id}">Edit</a>
            <a data-action="delete" data-id="${m.id}" style="color:${'var(--danger)'}">Delete</a>
          </td>
        `;
        membersTableBody.appendChild(tr);
      });
      populateIssueMemberSelect();
    }

    function renderIssues(){
      issuesTableBody.innerHTML = '';
      const active = transactions.filter(t => t.type==='issue' && !t.returned);
      active.forEach(t => {
        const b = books.find(x=>x.id===t.bookId) || {title:t.bookId};
        const m = members.find(x=>x.id===t.memberId) || {name:t.memberId};
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${t.id}</td>
          <td>${escapeHtml(b.title)}</td>
          <td>${escapeHtml(m.name)}</td>
          <td>${new Date(t.dueDate).toLocaleDateString()}</td>
          <td class="actions"><a data-action="return" data-id="${t.id}">Return</a></td>
        `;
        issuesTableBody.appendChild(tr);
      });
    }

    function populateIssueBookSelect(){
      issueBookSelect.innerHTML = '<option value="">-- choose --</option>';
      books.forEach(b => {
        const opt = document.createElement('option');
        opt.value = b.id;
        opt.textContent = `${b.title} — ${b.id} (Avail: ${getAvailableCopies(b.id)})`;
        issueBookSelect.appendChild(opt);
      });
    }
    function populateIssueMemberSelect(){
      issueMemberSelect.innerHTML = '<option value="">-- choose --</option>';
      members.forEach(m => {
        const opt = document.createElement('option');
        opt.value = m.id;
        opt.textContent = `${m.name} — ${m.id}`;
        issueMemberSelect.appendChild(opt);
      });
    }

    function escapeHtml(s){ return (s||'').toString().replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;'); }

    function persistAll(){
      write(KEY_BOOKS, books);
      write(KEY_MEMBERS, members);
      write(KEY_TX, transactions);
      renderAll();
    }

    function renderAll(){
      renderStats();
      renderBooks(searchBooksInput.value || '');
      renderMembers();
      renderIssues();
    }

    // BOOKS - add/update/delete
    bookForm.addEventListener('submit', function(ev){
      ev.preventDefault();
      const id = bookIdInput.value.trim();
      const obj = {
        id: id || genId('B'),
        title: bookTitle.value.trim(),
        author: bookAuthor.value.trim(),
        isbn: bookISBN.value.trim(),
        copies: Number(bookCopies.value) || 1,
        category: bookCategory.value.trim()
      };
      if(!obj.title){ alert('Book title required'); return; }
      if(id){
        books = books.map(b => b.id===id ? obj : b);
        notify('Book updated');
      } else {
        books.unshift(obj);
        notify('Book added');
      }
      bookForm.reset();
      bookIdInput.value = '';
      persistAll();
    });

    bookClearBtn.addEventListener('click', ()=>{ bookForm.reset(); bookIdInput.value=''; });
    btnNewBook.addEventListener('click', ()=>{ bookForm.reset(); bookIdInput.value=''; bookForm.scrollIntoView({behavior:'smooth'}); });

    booksTableBody.addEventListener('click', function(e){
      const a = e.target.closest('a');
      if(!a) return;
      const action = a.dataset.action;
      const id = a.dataset.id;
      if(action==='edit'){
        const b = books.find(x=>x.id===id);
        if(!b) return;
        bookIdInput.value = b.id; bookTitle.value = b.title; bookAuthor.value = b.author;
        bookISBN.value = b.isbn; bookCopies.value = b.copies; bookCategory.value = b.category;
        showView('books');
        bookForm.scrollIntoView({behavior:'smooth'});
      } else if(action==='delete'){
        if(!confirm('Delete this book?')) return;
        books = books.filter(x=>x.id!==id);
        transactions = transactions.filter(t=>t.bookId!==id);
        persistAll();
        notify('Book deleted');
      } else if(action==='issue'){
        issueBookSelect.value = id;
        showView('issue');
      }
    });

    searchBooksInput.addEventListener('input', ()=> renderBooks(searchBooksInput.value));

    function exportCSV(list, filename='export.csv'){
      if(!list || !list.length){ alert('Nothing to export'); return; }
      const keys = Object.keys(list[0]);
      const rows = [keys.join(','), ...list.map(obj => keys.map(k => `"${(obj[k] ?? '')}"`).join(','))].join('\n');
      const blob = new Blob([rows], {type:'text/csv'});
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a'); a.href = url; a.download = filename; a.click();
      URL.revokeObjectURL(url);
    }
    exportBooksBtn.addEventListener('click', ()=> exportCSV(books, 'books.csv'));

    // MEMBERS
    memberForm.addEventListener('submit', function(e){
      e.preventDefault();
      const id = memberIdInput.value.trim();
      const obj = { id: id || genId('M'), name: memberName.value.trim(), email: memberEmail.value.trim(), phone: memberPhone.value.trim() };
      if(!obj.name){ alert('Member name required'); return; }
      if(id){ members = members.map(m => m.id===id ? obj : m); notify('Member updated'); }
      else { members.unshift(obj); notify('Member added'); }
      memberForm.reset(); memberIdInput.value=''; persistAll();
    });
    memberClearBtn.addEventListener('click', ()=>{ memberForm.reset(); memberIdInput.value=''; });
    btnNewMember.addEventListener('click', ()=>{ memberForm.reset(); memberIdInput.value=''; memberForm.scrollIntoView({behavior:'smooth'}); });

    membersTableBody.addEventListener('click', function(e){
      const a = e.target.closest('a');
      if(!a) return;
      const action = a.dataset.action;
      const id = a.dataset.id;
      if(action==='edit'){
        const m = members.find(x=>x.id===id);
        if(!m) return;
        memberIdInput.value = m.id; memberName.value = m.name; memberEmail.value = m.email; memberPhone.value = m.phone;
        showView('members');
        memberForm.scrollIntoView({behavior:'smooth'});
      } else if(action==='delete'){
        if(!confirm('Delete this member? This will delete their transaction history.')) return;
        members = members.filter(x=>x.id!==id);
        transactions = transactions.filter(t=>t.memberId!==id);
        persistAll();
        notify('Member deleted');
      }
    });
    exportMembersBtn.addEventListener('click', ()=> exportCSV(members, 'members.csv'));

    // ISSUE & RETURN
    function issueBook(bookId, memberId, days=14){
      const book = books.find(b=>b.id===bookId);
      const member = members.find(m=>m.id===memberId);
      if(!book || !member) return alert('Invalid book or member');
      if(getAvailableCopies(bookId) < 1) return alert('No available copies to issue');
      const due = addDays(new Date(), days);
      const tx = { id: genId('T'), bookId, memberId, type:'issue', date:new Date().toISOString(), dueDate: due.toISOString(), returned:false };
      transactions.unshift(tx);
      persistAll();
      notify(`Issued "${book.title}" to ${member.name}. Due ${due.toLocaleDateString()}`);
    }
    function returnBook(txId){
      const tx = transactions.find(t=>t.id===txId);
      if(!tx) return;
      if(tx.type==='issue' && !tx.returned){
        tx.returned = true; tx.type = 'return'; tx.returnDate = new Date().toISOString();
        transactions = transactions.map(t => t.id===txId ? tx : t);
        persistAll();
        notify('Book returned');
      }
    }

    issue14Btn.addEventListener('click', ()=>{
      const b = issueBookSelect.value; const m = issueMemberSelect.value;
      if(!b || !m) return alert('Select both book and member');
      issueBook(b,m,14);
    });
    issue7Btn.addEventListener('click', ()=>{
      const b = issueBookSelect.value; const m = issueMemberSelect.value;
      if(!b || !m) return alert('Select both book and member');
      issueBook(b,m,7);
    });

    issuesTableBody.addEventListener('click', function(e){
      const a = e.target.closest('a');
      if(!a) return;
      const action = a.dataset.action; const id = a.dataset.id;
      if(action==='return'){ returnBook(id); }
    });

    // NAV & views
    function showView(name){
      for(const k in views){ views[k].style.display = (k===name ? '' : 'none'); }
      for(const k in navButtons){ navButtons[k].classList.toggle('active', k===name); }
      renderAll();
    }
    navButtons.dashboard.addEventListener('click', ()=> showView('dashboard'));
    navButtons.books.addEventListener('click', ()=> showView('books'));
    navButtons.members.addEventListener('click', ()=> showView('members'));
    navButtons.issue.addEventListener('click', ()=> showView('issue'));

    // initial render
    renderAll();

    // expose debug (optional)
    window._debug = { books, members, transactions, persistAll };

    // keyboard shortcuts
    document.addEventListener('keydown', (e)=>{
      if(e.ctrlKey && e.key==='b'){ showView('books'); }
      if(e.ctrlKey && e.key==='m'){ showView('members'); }
      if(e.ctrlKey && e.key==='i'){ showView('issue'); }
    });
  </script>
</body>
</html>
