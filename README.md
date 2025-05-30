// Basic data storage (localStorage)
const STORAGE_KEY = 'hajiHanifSonsData';

let data = JSON.parse(localStorage.getItem(STORAGE_KEY)) || {
  companyName: "Haji Hanif & Sons",
  logoText: "Haji Hanif & Sons",
  superAdminCNIC: "12345-1234567-1",
  users: {
    // cnic: { section: "Finance", name: "...", whatsapp: "..." }
  },
  sections: ["Admin", "Finance", "HR", "Workers", "Logistics", "Reception", "Store"],
  loggedInUser: null,
};

const loginForm = document.getElementById('loginForm');
const cnicInput = document.getElementById('cnicInput');
const pageTitle = document.getElementById('pageTitle');
const loginSection = document.getElementById('loginSection');
const dashboardSection = document.getElementById('dashboardSection');
const welcomeMsg = document.getElementById('welcomeMsg');
const navSections = document.getElementById('navSections');
const logoutBtn = document.getElementById('logoutBtn');
const logoTextElem = document.getElementById('logoText');

const adminSettingsSection = document.getElementById('adminSettingsSection');
const companyNameInput = document.getElementById('companyNameInput');
const logoTextInput = document.getElementById('logoTextInput');
const saveCompanyNameBtn = document.getElementById('saveCompanyNameBtn');
const saveLogoBtn = document.getElementById('saveLogoBtn');

const newSectionInput = document.getElementById('newSectionInput');
const addSectionBtn = document.getElementById('addSectionBtn');
const sectionsList = document.getElementById('sectionsList');
const contentArea = document.getElementById('contentArea');

function saveData() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
}

function updateUIForLogin() {
  loginSection.classList.remove('active');
  dashboardSection.classList.add('active');
  pageTitle.textContent = "Dashboard";
  welcomeMsg.textContent = `Welcome, CNIC: ${data.loggedInUser}`;
  logoTextElem.textContent = data.logoText;

  renderSections();
}

function renderSections() {
  navSections.innerHTML = '';
  sectionsList.innerHTML = '';

  data.sections.forEach(section => {
    const btn = document.createElement('button');
    btn.textContent = section;
    btn.onclick = () => showSectionContent(section);
    navSections.appendChild(btn);

    // List for admin management
    const li = document.createElement('li');
    li.textContent = section;

    const delBtn = document.createElement('button');
    delBtn.textContent = "Delete";
    delBtn.onclick = () => {
      if (confirm(`Delete section "${section}"?`)) {
        data.sections = data.sections.filter(s => s !== section);
        saveData();
        renderSections();
      }
    };

    li.appendChild(delBtn);
    sectionsList.appendChild(li);
  });

  if (data.loggedInUser === data.superAdminCNIC) {
    adminSettingsSection.classList.add('active');
    showAdminSettings();
  } else {
    adminSettingsSection.classList.remove('active');
    contentArea.innerHTML = `<p>Select a section to view content.</p>`;
  }
