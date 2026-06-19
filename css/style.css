// Initialize operational connection credentials for Supabase API
const SUPABASE_URL = "https://your-project-id.supabase.co"; // Replace with your real Supabase URL
const SUPABASE_ANON_KEY = "your-public-anon-key-here";      // Replace with your real Anon Key

const supabaseClient = supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

// State tracking logic for Modal toggle UI
function toggleAuthModal(showState) {
  document.getElementById('authModal').style.display = showState ? 'flex' : 'none';
}

function switchTab(targetTab) {
  const isLogin = targetTab === 'login';
  document.getElementById('loginForm').style.display = isLogin ? 'block' : 'none';
  document.getElementById('registerForm').style.display = isLogin ? 'none' : 'block';
  
  document.getElementById('tabLogin').classList.toggle('active', isLogin);
  document.getElementById('tabRegister').classList.toggle('active', !isLogin);
}

// 1. Handle Registration Pipeline (Sign Up)
async function handleSignUp(event) {
  event.preventDefault();
  
  const email = document.getElementById('regEmail').value;
  const password = document.getElementById('regPassword').value;
  const fullName = document.getElementById('regFullName').value;

  try {
    const { data: authData, error: authError } = await supabaseClient.auth.signUp({
      email: email,
      password: password,
    });

    if (authError) throw authError;

    if (authData.user) {
      // Sync detailed public info to custom profiles table
      const { error: profileError } = await supabaseClient
        .from('profiles')
        .insert([{ id: authData.user.id, full_name: fullName, email: email }]);

      if (profileError) throw profileError;

      alert("Registration request processed! Check your email inbox to verify your account. ✉️");
      toggleAuthModal(false);
    }
  } catch (error) {
    alert(`Registration system error: ${error.message}`);
  }
}

// 2. Handle Login Pipeline (Sign In)
async function handleSignIn(event) {
  event.preventDefault();

  const email = document.getElementById('loginEmail').value;
  const password = document.getElementById('loginPassword').value;

  try {
    const { data, error } = await supabaseClient.auth.signInWithPassword({
      email: email,
      password: password,
    });

    if (error) throw error;

    alert(`Welcome back! Connection successfully authorized. 👋`);
    toggleAuthModal(false);
    checkSessionState(); // Reload UI components
  } catch (error) {
    alert(`Authentication system error: ${error.message}`);
  }
}

// 3. Keep Track of Active Session State
async function checkSessionState() {
  const authHeaderContainer = document.getElementById('authHeader');
  if (!authHeaderContainer) return;

  const { data: { session } } = await supabaseClient.auth.getSession();

  if (session) {
    authHeaderContainer.innerHTML = `
      <span style="margin-right: 15px; font-weight:500;">👤 Connected</span>
      <button class="btn" style="color:red; border-color:red;" onclick="handleSignOut()">Logout</button>
    `;
  } else {
    authHeaderContainer.innerHTML = `
      <button class="btn" onclick="toggleAuthModal(true)">Sign In / Register</button>
    `;
  }
}

// 4. Handle Disconnection Pipeline (Sign Out)
async function handleSignOut() {
  await supabaseClient.auth.signOut();
  alert("Session terminated successfully.");
  window.location.reload();
}

// Automatically scan for cookies/session state on page boot execution
document.addEventListener("DOMContentLoaded", checkSessionState);
