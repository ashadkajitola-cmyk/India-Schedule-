<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cricket Central - Admin Dashboard & Subscriptions</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-slate-950 text-slate-100 flex flex-col font-sans min-h-screen">
  <div id="root">
    <!-- Header Navbar -->
    <header class="bg-slate-900 border-b border-slate-800 sticky top-0 z-50 px-4 py-3 flex items-center justify-between shadow-md">
      <div class="flex items-center space-x-3 cursor-pointer">
        <span class="text-xl">←</span>
        <span class="font-bold text-lg tracking-wide text-transparent bg-clip-text bg-gradient-to-r from-blue-400 to-indigo-400">Cricket Central</span>
      </div>
      <div class="flex items-center space-x-2">
        <button onclick="switchTab('matches')" id="tabMatches" class="px-3 py-1.5 text-xs font-semibold rounded-lg bg-blue-600 text-white shadow">MATCHES</button>
        <button onclick="switchTab('points')" id="tabPoints" class="px-3 py-1.5 text-xs font-semibold rounded-lg bg-slate-800 text-slate-300 hover:bg-slate-700">POINTS TABLE</button>
        <button onclick="switchTab('groups')" id="tabGroups" class="px-3 py-1.5 text-xs font-semibold rounded-lg bg-slate-800 text-slate-300 hover:bg-slate-700">GROUPS</button>
        <button onclick="openSubscriptionModal()" class="px-3 py-1.5 text-xs font-semibold rounded-lg bg-amber-600 text-white shadow hover:bg-amber-500">⭐ Subscription</button>
      </div>
    </header>

    <!-- Subscription Modal Popup -->
    <div id="subModal" class="fixed inset-0 bg-black/80 z-50 flex items-center justify-center p-4 hidden">
      <div class="bg-slate-900 border border-slate-800 rounded-2xl p-6 max-w-md w-full shadow-2xl relative">
        <h3 class="text-xl font-bold text-amber-400 mb-2">⭐ Choose Subscription Plan</h3>
        <p class="text-xs text-slate-400 mb-4">Payments processed via Google Play Balance to developer account: <strong class="text-slate-200">Ashadkazitola@gmail.com</strong></p>
        
        <!-- Free verification for 9569981484 -->
        <div class="mb-4 p-3 bg-slate-950 border border-slate-700 rounded-xl">
          <label class="block text-xs font-medium text-slate-300 mb-1">Free Access Number (9569981484):</label>
          <div class="flex gap-2">
            <input type="text" id="freeNumberInput" placeholder="Enter Mobile Number" class="bg-slate-900 border border-slate-700 px-3 py-1.5 rounded-lg w-full text-sm text-slate-200">
            <button onclick="verifyFreeNumber()" class="bg-emerald-600 hover:bg-emerald-500 px-4 py-1.5 rounded-lg text-xs font-bold text-white">Verify</button>
          </div>
          <p id="freeMsg" class="text-xs mt-1 text-emerald-400 hidden">✔ Free Access Granted for 9569981484!</p>
        </div>

        <div class="space-y-3 mb-6">
          <label class="flex items-center justify-between p-3 bg-slate-950 border border-slate-800 rounded-xl cursor-pointer hover:border-amber-500">
            <div>
              <span class="font-bold text-sm block text-slate-200">Weekly Plan</span>
              <span class="text-xs text-slate-400">Play Store Billing</span>
            </div>
            <span class="text-amber-400 font-bold">₹15</span>
            <input type="radio" name="subPlan" value="15" class="accent-amber-500" checked>
          </label>

          <label class="flex items-center justify-between p-3 bg-slate-950 border border-slate-800 rounded-xl cursor-pointer hover:border-amber-500">
            <div>
              <span class="font-bold text-sm block text-slate-200">Half Monthly Plan</span>
              <span class="text-xs text-slate-400">Play Store Billing</span>
            </div>
            <span class="text-amber-400 font-bold">₹22</span>
            <input type="radio" name="subPlan" value="22" class="accent-amber-500">
          </label>

          <label class="flex items-center justify-between p-3 bg-slate-950 border border-slate-800 rounded-xl cursor-pointer hover:border-amber-500">
            <div>
              <span class="font-bold text-sm block text-slate-200">Yearly Plan (Early)</span>
              <span class="text-xs text-slate-400">Play Store Billing</span>
            </div>
            <span class="text-amber-400 font-bold">₹399</span>
            <input type="radio" name="subPlan" value="399" class="accent-amber-500">
          </label>
        </div>

        <div class="flex gap-2">
          <button onclick="payViaPlayStore()" class="w-full bg-amber-600 hover:bg-amber-500 py-2.5 rounded-xl text-sm font-bold shadow text-white flex items-center justify-center gap-2">
            <span>Pay with Play Balance</span>
          </button>
          <button onclick="closeSubscriptionModal()" class="bg-slate-800 hover:bg-slate-700 px-4 py-2.5 rounded-xl text-sm font-medium text-slate-300">Close</button>
        </div>
      </div>
    </div>

    <!-- Main Container -->
    <div class="p-4 max-w-4xl mx-auto w-full flex-grow">
      
      <!-- Admin Dashboard Section -->
      <div class="bg-slate-900 border border-slate-800 rounded-2xl p-6 shadow-xl mb-6">
        <h3 class="text-xl font-bold mb-4 text-indigo-400 flex items-center gap-2">Admin Dashboard</h3>
        
        <div id="loginSection" class="mb-6 flex gap-2">
          <input type="password" id="adminPass" placeholder="Enter Admin Secret Password..." class="bg-slate-950 border border-slate-700 px-4 py-2 rounded-xl w-full text-sm focus:outline-none focus:border-indigo-500 text-slate-200">
          <button onclick="adminLogin()" class="bg-indigo-600 hover:bg-indigo-500 px-5 py-2 rounded-xl text-sm font-bold shadow text-white">Login</button>
        </div>

        <div id="adminPanel" class="space-y-6">
          <div class="p-4 bg-emerald-950/30 border border-emerald-800/50 rounded-xl">
            <p class="text-emerald-400 font-semibold mb-4 text-sm">✔ Logged In Successfully</p>
            
            <!-- 1. Create Tournament Group -->
            <div class="space-y-4">
              <h4 class="font-bold text-sm text-slate-200">📂 1. Create Tournament Group</h4>
              <div>
                <label class="block text-xs font-medium text-slate-400 mb-1">Format</label>
                <select id="groupFormat" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-sm text-slate-200">
                  <option value="test">Test (WTC)</option>
                  <option value="odi">ODI</option>
                  <option value="t20">T20I</option>
                </select>
              </div>
              <div>
                <label class="block text-xs font-medium text-slate-400 mb-1">Group Name</label>
                <input type="text" id="groupNameInput" placeholder="Enter Group Name (e.g. Group A)" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-sm text-slate-200">
              </div>
              <button onclick="checkSubscriptionBeforeAction('Create Tournament Group')" class="w-full bg-emerald-600 hover:bg-emerald-500 py-2 rounded-xl text-sm font-bold shadow text-white">Create Group</button>
            </div>

            <hr class="border-slate-800 my-6">

            <!-- 2. Add Match Schedule -->
            <div class="space-y-4">
              <h4 class="font-bold text-sm text-slate-200">📅 2. Add Match Schedule</h4>
              <div>
                <label class="block text-xs font-medium text-slate-400 mb-1">Series Type</label>
                <select class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-sm text-slate-200">
                  <option value="existing">Usi Series ka match hai</option>
                  <option value="new">New Series ka match hai</option>
                </select>
              </div>
              <div>
                <label class="block text-xs font-medium text-slate-400 mb-1">Match Details / Teams</label>
                <input type="text" id="matchDetailsInput" placeholder="e.g. India vs Australia" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-sm text-slate-200">
              </div>
              <button onclick="checkSubscriptionBeforeAction('Add Match Schedule')" class="w-full bg-indigo-600 hover:bg-indigo-500 py-2 rounded-xl text-sm font-bold shadow text-white">Add Match</button>
            </div>

            <hr class="border-slate-800 my-6">

            <!-- 3. Update Global Rankings -->
            <div class="space-y-4">
              <h4 class="font-bold text-sm text-slate-200">⚡ 3. Update Global Rankings</h4>
              <div>
                <label class="block text-xs font-medium text-slate-400 mb-1">Format</label>
                <select class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-sm text-slate-200">
                  <option value="test">Test (WTC)</option>
                  <option value="odi">ODI</option>
                  <option value="t20">T20I</option>
                </select>
              </div>
              <div>
                <label class="block text-xs font-medium text-slate-400 mb-1">Match Result Outcome</label>
                <select class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-sm text-slate-200">
                  <option value="team1">Team 1 Wins</option>
                  <option value="team2">Team 2 Wins</option>
                  <option value="draw">Draw / Tie</option>
                  <option value="nr">No Result / Abandoned</option>
                </select>
              </div>
              <button onclick="checkSubscriptionBeforeAction('Update Global Rankings')" class="w-full bg-amber-600 hover:bg-amber-500 py-2 rounded-xl text-sm font-bold shadow text-white">Apply Global Match Result</button>
            </div>

            <hr class="border-slate-800 my-6">

            <!-- 4. Update Custom Group Points Table -->
            <div class="space-y-4">
              <h4 class="font-bold text-sm text-slate-200">🏆 4. Update Custom Group Points Table</h4>
              <div>
                <input type="number" placeholder="Team 1 Score / Overs" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-sm text-slate-200 mb-2">
                <input type="number" placeholder="Team 2 Score / Overs" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-sm text-slate-200">
              </div>
              <button onclick="checkSubscriptionBeforeAction('Update Custom Points Table')" class="w-full bg-indigo-600 hover:bg-indigo-500 py-2 rounded-xl text-sm font-bold shadow text-white">Update Group Points</button>
            </div>

            <hr class="border-slate-800 my-6">

            <!-- 5. Set Group Qualification Rules -->
            <div class="space-y-4">
              <h4 class="font-bold text-sm text-slate-200">🎯 5. Set Group Qualification Rules</h4>
              <div>
                <label class="block text-xs font-medium text-slate-400 mb-1">Top Teams to Qualify</label>
                <input type="number" placeholder="e.g. 2" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-3 py-2 text-sm text-slate-200">
              </div>
              <button onclick="checkSubscriptionBeforeAction('Set Qualification Rules')" class="w-full bg-purple-600 hover:bg-purple-500 py-2 rounded-xl text-sm font-bold shadow text-white">Save Rules & Generate Next Stage</button>
            </div>

            <hr class="border-slate-800 my-6">

            <!-- 6. Reset WTC Test Standings -->
            <div class="space-y-4">
              <h4 class="font-bold text-sm text-slate-200">🔄 6. Reset WTC Test Standings</h4>
              <button onclick="checkSubscriptionBeforeAction('Reset WTC Standings')" class="w-full bg-rose-600 hover:bg-rose-500 py-2 rounded-xl text-sm font-bold shadow text-white">Reset WTC Table</button>
            </div>
          </div>
        </div>
      </div>

      <!-- Original Site Fixtures & Tables Display Section -->
      <div class="space-y-6">
        <div class="bg-slate-900 border border-slate-800 rounded-2xl p-6 shadow-xl">
          <h4 class="font-bold text-lg mb-4 text-slate-200">Team Fixtures & Standings</h4>
          <div class="overflow-x-auto">
            <table class="w-full text-left text-sm text-slate-300">
              <thead class="bg-slate-950 uppercase text-xs text-slate-400 border-b border-slate-800">
                <tr><th class="p-3">POS</th><th class="p-3">TEAM</th><th class="p-3">P</th><th class="p-3">W</th><th class="p-3">L</th><th class="p-3">D</th><th class="p-3">PTS</th><th class="p-3">PCT</th></tr>
              </thead>
              <tbody id="standingsTableBody">
                <tr class="border-b border-slate-800"><td class="p-3">1</td><td class="p-3 font-semibold text-white">India</td><td class="p-3">12</td><td class="p-3">9</td><td class="p-3">2</td><td class="p-3">1</td><td class="p-3">100</td><td class="p-3">69.4%</td></tr>
                <tr class="border-b border-slate-800"><td class="p-3">2</td><td class="p-3 font-semibold text-white">Australia</td><td class="p-3">12</td><td class="p-3">8</td><td class="p-3">3</td><td class="p-3">1</td><td class="p-3">92</td><td class="p-3">63.8%</td></tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

    </div>
  </div>

  <!-- JavaScript Application Logic -->
  <script>
    let isSubscribed = false;

    function switchTab(tabName) {
      document.getElementById('tabMatches').className = "px-3 py-1.5 text-xs font-semibold rounded-lg bg-slate-800 text-slate-300 hover:bg-slate-700";
      document.getElementById('tabPoints').className = "px-3 py-1.5 text-xs font-semibold rounded-lg bg-slate-800 text-slate-300 hover:bg-slate-700";
      document.getElementById('tabGroups').className = "px-3 py-1.5 text-xs font-semibold rounded-lg bg-slate-800 text-slate-300 hover:bg-slate-700";
      
      if(tabName === 'matches') document.getElementById('tabMatches').className = "px-3 py-1.5 text-xs font-semibold rounded-lg bg-blue-600 text-white shadow";
      if(tabName === 'points') document.getElementById('tabPoints').className = "px-3 py-1.5 text-xs font-semibold rounded-lg bg-blue-600 text-white shadow";
      if(tabName === 'groups') document.getElementById('tabGroups').className = "px-3 py-1.5 text-xs font-semibold rounded-lg bg-blue-600 text-white shadow";
    }

    function adminLogin() {
      const pass = document.getElementById('adminPass').value;
      if(pass.trim() !== "") {
        alert("Logged In Successfully!");
      } else {
        alert("Please enter admin password.");
      }
    }

    function openSubscriptionModal() {
      document.getElementById('subModal').classList.remove('hidden');
    }

    function closeSubscriptionModal() {
      document.getElementById('subModal').classList.add('hidden');
    }

    function verifyFreeNumber() {
      const num = document.getElementById('freeNumberInput').value.trim();
      if (num === '9569981484') {
        isSubscribed = true;
        document.getElementById('freeMsg').classList.remove('hidden');
        setTimeout(() => {
          closeSubscriptionModal();
          alert('Free Access Active for number 9569981484!');
        }, 800);
      } else {
        alert('Invalid Free Number! Please choose a subscription plan.');
      }
    }

    function payViaPlayStore() {
      const selectedPlan = document.querySelector('input[name="subPlan"]:checked').value;
      const confirmPay = confirm(`Pay ₹${selectedPlan} using Google Play Balance?\nFunds will be credited to developer account: Ashadkazitola@gmail.com`);
      if (confirmPay) {
        isSubscribed = true;
        alert('Payment Successful via Play Store Balance! Subscription activated.');
        closeSubscriptionModal();
      }
    }

    function checkSubscriptionBeforeAction(actionName) {
      if (!isSubscribed) {
        alert('⚠️ Active subscription required to perform "' + actionName + '". Please subscribe or use free number (9569981484).');
        openSubscriptionModal();
      } else {
        alert('Success! Schedule/Action completed for: ' + actionName);
      }
    }
  </script>
</body>
</html>
